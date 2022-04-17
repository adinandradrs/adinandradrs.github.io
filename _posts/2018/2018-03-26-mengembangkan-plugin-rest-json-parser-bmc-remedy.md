---
title: Mengembangkan Plugin REST JSON Parser BMC Remedy
author:
  name: Adinandra Dharmasurya
  link: https://adinandra.dharmasurya.id
date: 2018-03-26 +0700
categories: [Pemrograman]
tags: [remedy,arsys,custom]
pin: false
image:
  src: /etc/scripting.png
  width: 350   # in pixels
  height: 350   # in pixels
  alt: Scripting
---

Berhubung sebentar lagi saya akan resign tidak ada salahnya meninggalkan jejak untuk mencatat sebagian kecil ilmu yang telah didapatkan selama di Mitra Integrasi Informatika (MII). Di MII saya banyak sekali belajar secara otodidak untuk mengembangkan ilmu serta aplikasi yang ada diluar kemampuan aslinya. Hal tersebut dikarenakan memang dasarnya saya memulai karir sebagai pengembang aplikasi.

Kebetulan BMC Remedy platformnya based on Java. Untuk melakukan kustomnya tambahan fitur pada komponennya pun seperti WordPress, yaitu dengan menggunakan plugin. Plugin yang siang ini saya adalah plugin untuk consume RESTful Web Service dari eksternal. Apakah mengembakan sebuah plugin untuk BMC Remedy itu susah? Jujur saja tidak sama sekali.

Pertama yang saya butuhkan adalah 2 pustaka API dari BMC Remedy AR System. Karena saya menggunakan AR System 9.1 maka pustaknya adalah arpluginsvr91_build002.jar, arapi91_build002.jar, dan json-simple-1.1.1.jar. Setelah itu backup terlebih dulu beberapa file config antara lain AR.conf dan pluginsvr_config.xml.

Use casenya semisal saya memiliki Mid Form untuk menampung hasil consume web service dengan nama BMC:Learning:JSON. Response JSON yang saya tarik berasal dari ```http://domain/xxxxx.json``` dengan method GET. Response bodynya katakanlah seperti di bawah ini
```
[
    {"Programming Language" : "Java"},
    {"Programming Language" : "C#"},
    {"Programming Language" : "PHP"},
    {"Programming Language" : "Python"}
]
```

Data pada field Programming Language akan disimpan ke Short Description pada Mid Form BMC:Learning:JSON. Sehingga nanti pada filter akan memiliki satu action yaitu "Set Field" dengan sourcenya adalah plugin dengan 3 parameter yaitu nama form, mapping field, dan alamat URL web service RESTful yang ingin diconsume. Kok terkesan banyak parameternya? Agar sedikit lebih dinamis tentunya dan tidak terlalu hardcode sehingga lebih reuseable serta mudah dikembangkan di kemudian hari.

Next buat New Java Project di Eclipse dan beri nama JSONParser. Dari project tersebut referensikan 3 pustaka di atas. Dan berikut adalah penjelasan dari setiap pustakanya

1. arpluginvr91_build002.jar adalah pustaka untuk mengimplementasikan API plugin di komponen filter AR System.
2. arapi91_build002.jar adalah pustaka untuk koneksi ke AR System.
3. json-simple-1.1.1.jar adalah pustaka untuk encode dan decode JSON.

Untuk file berikut adalah penjelasannya
1. AR.conf adalah file konfigurasi AR System beserta seluruh plugin.
2. pluginsvr_config.xml adalah file konfigurasi plugin dan path class yang akan saya gunakan.

Lalu saya membuat sebuah class dengan nama JSONParsePlugin berikut snippetnya


```
package plugin.adinandra.dharmasurya.net;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;
import org.json.simple.JSONArray;
import org.json.simple.JSONObject;
import org.json.simple.parser.JSONParser;
import org.json.simple.parser.ParseException;
import com.bmc.arsys.pluginsvr.plugins.ARFilterAPIPlugin;
import com.bmc.arsys.pluginsvr.plugins.ARPluginContext;
import com.bmc.arsys.api.ARException;
import com.bmc.arsys.api.ARServerUser;
import com.bmc.arsys.api.Entry;
import com.bmc.arsys.api.Value;

public class JSONParserPlugin extends ARFilterAPIPlugin {

	@Override
	public List filterAPICall(ARPluginContext context, List listOfParam) throws ARException {
		String serverName = context.getARConfigEntry("Server-Connect-Name");
		int port = Integer.valueOf(context.getARConfigEntry("TCD-Specific-Port"));
		ARServerUser server = new ARServerUser(context, "", serverName);
		server.setPort(port);
		String formName = listOfParam.get(0).getValue().toString();
		String fieldId = listOfParam.get(1).getValue().toString();
		String urlStr = listOfParam.get(2).getValue().toString();
		String jsonStr = this.doGetJSON(urlStr);
		List results = new ArrayList();
		String [] fieldIds = fieldId.split(",");
		JSONParser jParser = new JSONParser();
		Object json = null;
		try {
			json = jParser.parse(jsonStr);
		} catch (ParseException e) {
			e.printStackTrace();
		}
		JSONArray jArr = (JSONArray) json;
		for(int i = 0; i < jArr.size(); i++) {
			JSONObject jObj = (JSONObject) jArr.get(i);
			int fieldIdx = 0;
			Entry entry = new Entry();
			for(Iterator iterator = jObj.keySet().iterator(); iterator.hasNext();) {
				String key = (String) iterator.next();
				String value = jObj.get(key).toString();
				int field = Integer.valueOf(fieldIds[fieldIdx]);
				entry.put(field, new Value(value));
				fieldIdx++;
			}
			String id = server.createEntry(formName, entry);
			results.add(new Value(id));
		}
		return results;
	}

	private String doGetJSON(String urlStr) {
		String jsonStr = "";
		try {
			URL url = new URL(urlStr);
			HttpURLConnection urlCn = (HttpURLConnection) url.openConnection();
			urlCn.setDoOutput(true);
			urlCn.setRequestMethod("GET");
			BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(urlCn.getInputStream()));
			String line;
			StringBuilder content = new StringBuilder();
			while((line = bufferedReader.readLine())!= null) {
				content.append(line);
				content.append(System.lineSeparator());
			}
			jsonStr = content.toString();
		} catch (Exception e1) {
			// TODO Auto-generated catch block
			e1.printStackTrace();
		}
		return jsonStr;
	}

}
```

Setelah itu saya harus modifikasi AR.conf dan pluginsvr_conf.xml sebagai berikut.

**PLUGINSVR_CONF.XML**
```
<plugins>
    <name>ADINANDRA.DHARMASURYA.JSONPARSER</name>
    <classname>plugin.adinandra.dharmasurya.net.JSONParserPlugin </classname>
    <filename>{your AR Server location}\pluginsvr\custom\jsonparser.jar</filename>
    <pathelement type="path">{your AR Server location}\pluginsvr\custom</pathelement>
    <pathelement type="location">{your ar server location}\pluginsvr\custom\lib\json-simple-1.1.1.jar</pathelement>
    <userDefined>
        <bulk_transaction_size>32</bulk_transaction_size>
        <server_name>{your hostname}</server_name>
        <use_bulk_transaction>false</use_bulk_transaction>
        <server_port>{your RPC port}</server_port>
    </userDefined>
    ......
    ......
</plugins>
```

**AR.CONF**
```
Server-Plugin-Alias: ADINANDRA.DHARMASURYA.JSONPARSER ADINANDRA.DHARMASURYA.JSONPARSER :9999
```

Selesai config lalu simpan, selanjutnya export Java Project Eclipse menjadi sebuah executable jar dengan nama jsonparser.jar dan tempatkan di {lokasi instalasi AR Server}\pluginsvr\custom\
> buat folder dengan nama custom jika tidak ada 

Untuk tambahan eksternal library di dalam folder custom buat satu lagi folder dengan nama lib dan copy library json-simple-1.1.1.jar ke dalam folder lib. Karena berbasis interface ```ARFilterAPIPlugin``` saya diwajibkan untuk mengimplementasikan method ```filterAPICall (context, filter parameter)```. Filter parameter berasal dari inputan yang ada di filter nanti. Sehingga pada bagian awal terdapat variabel nama form, field id, dan URL yang nilainya diset secara otomatis oleh action "Set Field". 

Method ```doGetJSON``` adalah sebuah prosedur yang digunakan untuk menarik JSON dari URL. Returnnya berupa string dan akan diproses dengan library json-simple. Setelah selesai semuanya maka harus restart AR System dan baru setelah itu dapat dicoba pada filter. Pada filter yang butuh untuk memanggil plugin, nama pluginnya adalah ADINANDRA.DHARMASURYA.JSONPARSER. Saya harus set actionnya seperti di bawah ini.

1. ```$0$``` set valuenya dengan nama form yaitu BMC:Learning:JSON
2. ```$1$``` set valuenya dengan mapping field dari return JSON dengan yang ada di form. Karena sudah disepakati field "Programming Language" akan masuk sebagai short description maka dapat set valuenya menjadi "8" sesuai dengan ID field dari Short Description. Jika semisal memiliki lebih dapat menggunakan separator koma "," semisal ```"8,5110001,51100002, dst"```
3. ```$2$``` set dengan alamat url RESTful semisal "http://domain/xxxxx.json"

Semoga saya dapat mencoba kembali BMC Remedy lagi jika ada kesempatan. Semoga bermanfaat, salam.