---
title: Eksekusi Store Procedure dengan Java
author:
  name: Adinandra Dharmasurya
  link: https://adinandra.dharmasurya.id
date: 2015-07-25 +0700
categories: [Pemrograman]
tags: [sql,java,database]
pin: false
mermaid: true
image:
  src: /etc/scripting.png
  width: 350   # in pixels
  height: 350   # in pixels
  alt: Scripting
---

Berhubung minggu malam ini sedang hujan deras dan tidak dapat keluar dari kost, akhirnya saya iseng membuka IDE Eclipse yang lama tidak tersentuh. Tema coding pada tulisan ini adalah eksekusi stored procedure menggunakan Java. Java memiliki kemampuan untuk eksekusi store procedure karena didukung oleh pustaka JDBC.

Untuk sample saya menggunakan DBMS SQL Server 2008R2 dengan library JTDs. Untuk mengeksekusinya saya menggunakan class Callable Statement dan bukan Prepared Statement. Kenapa menggunakan Callable Statement? Karena object yang dieksekusi store procedure sudah tercompile di dalam DBMS dan bukan perintah mentah seperti query DML. Selain itu Callable memang didesain untuk mengeksekusi store procedure karena memiliki terdapat subset yang dapat handle parameter out dan in. Berikut adalah snippetnya stored procedurenya :

```
USE [AppDB]
GO
/****** Object:  StoredProcedure [dbo].[sp_name]    Script Date: 25/07/2015 21:35:13 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
-- =============================================
-- Author:		nandcep
-- Create date: 25-07-2015
-- Description:	example only
-- =============================================
CREATE PROCEDURE [dbo].[sp_name] 
	-- Add the parameters for the stored procedure here
	@in_id varchar(50)
AS
BEGIN
	-- SET NOCOUNT ON added to prevent extra result sets from
	-- interfering with SELECT statements.
	SET NOCOUNT ON;

    -- Insert statements for procedure here
	SELECT data from dummy
	where id = @in_id; 
END
```
Baris kode di atas adalah store procedure yang akan dipanggil di dalam Java. Berikut snippet code Java untuk pemanggilan store procedure di atas dengan menggunakan object dari class ```CallableStatement``` :

```
try{
    CallableStatement callableStatement = DBConnection.getConnection().prepareCall("EXEC sp_name ?"); //execute sp dengan callable, parameter diwakilkan sebagai ?
	callableStatement.setEscapeProcessing(true);
	callableStatement.setString(1, "IDX0123ZZYY"); //parameter beserta indexnya
	executed = callableStatement.execute(); //eksekusi
	ResultSet resultSet = callableStatement.getResultSet();
	while(resultSet.next()){
		data = resultSet.getString("data"); //mendapatkan data yang dihasilkan oleh store procedure (lihat baris terakhir)
	}
}
catch(Exception e){
	throw new BaseError(DBConnectionConstant.ERR_SQL_NOT_EXECUTE);
}
if(!executed){
	throw new BaseError(DBConnectionConstant.ERR_SQL_NOT_EXECUTE);
}
```

Parameter yang dipassing dari Java ke parameter store procedure ```in_id``` menggunakan set parameter berdasarkan indeks beserta tipe datanya. Pada contoh di atas parameter menggunakan tipe data berupa string dan ada di urutan pertama. Untuk tahapan dan penjelasan saya telah berikan dan ada pada bagian komentar. Semoga bermanfaat dan terimakasih.

*Disadur dari blog lama saya di WordPress.com*