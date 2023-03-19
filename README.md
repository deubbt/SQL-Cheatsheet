<center>### BBT - AI komitesi</center>

# SQL Hatırlatma Kağıdı

Bu depoda, kullanım örnekleriyle birlikte tüm ilgili SQL sorgularının hatırlatması bulunmaktadır.

İyi kullanımlar!

# İçindekiler 
1. [Veri Bulma Sorguları.](#bul)
2. [Veri Değiştirme Sorguları.](#degistir)
3. [Raporlama Sorguları.](#rapor)
4. [Birleştirme Sorguları.](#birlesim)
5. [Görünüm Sorguları.](#gorunum)
6. [Tablo Değiştirme Sorguları.](#degistirme)
7. [Tablo Oluşturma Sorgusu.](#olustur)
8. [Yorum Sorgusu.](#yorum)
9. [Tetikleyici Sorgusu.](#tetikleyici)
10. [Saklı Prosedür.](#prosedür)
11. [Sorgu İndexleme.](#index)


<a name="bul"></a>
# 1. Veri Bulma Sorguları

### **SELECT**: Veritabanından veri seçmek için kullanılır
* `SELECT` * `FROM` tablo_adi;

### **DISTINCT**: Yinelenen değerleri filtreler ve belirtilen sütunun satırlarını döndürür
* `SELECT DISTINCT` sütun_adi;

### **WHERE**: Kayıtları/satırları filtrelemek için kullanılır
* `SELECT` sütun1, sütun2 `FROM` tablo_adi `WHERE` kosul;
* `SELECT` * `FROM` tablo_adi `WHERE` kosul1 `AND` kosul2;
* `SELECT` * `FROM` tablo_adi `WHERE` kosul1 `OR` kosul2;
* `SELECT` * `FROM` tablo_adi `WHERE NOT` kosul;
* `SELECT` * `FROM` tablo_adi `WHERE` kosul1 `AND` (kosul2 `OR` kosul3);
* `SELECT` * `FROM` tablo_adi `WHERE EXISTS` (`SELECT` sütun_adi `FROM` tablo_adi `WHERE` kosul);

### **ORDER BY**: Sonuç kümesini artan veya azalan sırada sıralamak için kullanılır
* `SELECT` * `FROM` tablo_adi `ORDER BY` sütun;
* `SELECT` * `FROM` tablo_adi `ORDER BY` sütun `DESC`;
* `SELECT` * `FROM` tablo_adi `ORDER BY` sütun1 `ASC`, sütun2 `DESC`;

### **SELECT TOP**: Tablonun üstünden döndürülecek kayıt sayısını belirtmek için kullanılır
* `SELECT TOP` sayi sütun_adi `FROM` tablo_adi `WHERE` kosul;
* `SELECT TOP` sayi Percent sütun_adi `FROM` tablo_adi `WHERE` kosul;
* Tüm veritabanı sistemleri `SELECT TOP`'ı desteklemez. MySQL eşdeğeri `LIMIT` ifadesidir.
* `SELECT` sütun_adi `FROM` tablo_adi `LIMIT` atlanacak_satır_sayısı, kayıt_sayısı;


### **LIKE**: Bir sütundaki belirli bir deseni aramak için WHERE koşulunda kullanılan operatördür.
* % (yüzde işareti) sıfır, bir veya çok karakteri temsil eden bir joker karakteridir.
* _ (alt çizgi) tek bir karakteri temsil eden bir joker karakteridir.
* `SELECT` sütun_isimleri `FROM` tablo_adı `WHERE` sütun_isim `LIKE` desen;
* `LIKE` ‘a%’ (“a” ile başlayan herhangi bir değeri bul)
* `LIKE` ‘%a’ (“a” ile biten herhangi bir değeri bul)
* `LIKE` ‘%or%’ (herhangi bir pozisyonda “or” içeren herhangi bir değeri bul)
* `LIKE` ‘_r%’ (ikinci pozisyonda “r” içeren herhangi bir değeri bul)
* `LIKE` ‘a_%_%’ (“a” ile başlayan ve en az 3 karakter uzunluğunda olan herhangi bir değeri bul)
* `LIKE` ‘[a-c]%’ (“a”, “b” veya “c” ile başlayan herhangi bir değeri bul)

### **IN**: Bir WHERE koşulunda birden fazla değer belirtmenize olanak tanıyan bir operatördür.
* Temel olarak IN operatörü, birden çok OR koşulunun kısaltmasıdır.
* `SELECT` sütun_isimleri `FROM` tablo_adı `WHERE` sütun_isim `IN` (değer1, değer2, …);
* `SELECT` sütun_isimleri `FROM` tablo_adı `WHERE` sütun_isim `IN` (`SELECT` sorgusu);

### **BETWEEN**: Verilen aralık içindeki değerleri seçen bir operatördür (dahil).
* `SELECT` sütun_isimleri `FROM` tablo_adı `WHERE` sütun_isim `BETWEEN` değer1 `AND` değer2;
* `SELECT` * `FROM` Ürünler `WHERE` (sütun_isim `BETWEEN` değer1 `AND` değer2) `AND NOT` sütun_isim2 `IN` (değer3, değer4);
* `SELECT` * `FROM` Ürünler `WHERE` sütun_isim `BETWEEN` #01/07/1999# AND #03/12/1999#;


### **NULL**: Bir alanın değeri olmayan değerleri
* `SELECT` * `FROM` tablo_adı `WHERE` sütun_adı `IS NULL`;
* `SELECT` * `FROM` tablo_adı `WHERE` sütun_adı `IS NOT NULL`;

### **AS**: takma adlar, geçici olarak bir tabloya veya sütuna bir ad atamak için kullanılır
* `SELECT` sütun_adı `AS` takma_ad `FROM` tablo_adı;
* `SELECT` sütun_adı `FROM` tablo_adı `AS` takma_ad;
* `SELECT` sütun_adı1 `AS` takma_ad1, sütun_adı2 `AS` takma_ad2;
* `SELECT` sütun_adı1, sütun_adı2 + ‘, ‘ + sütun_adı3 `AS` takma_ad;

### **UNION**: İki veya daha fazla SELECT ifadesinin sonuç kümesini birleştirmek için kullanılan bir küme işlemcisidir.
* UNION içindeki her SELECT ifadesinde aynı sayıda sütun olmalıdır
* Sütunlar benzer veri tiplerine sahip olmalıdır
* Her SELECT ifadesindeki sütunlar aynı sırada olmalıdır
* `SELECT` sütunlar_adı `FROM` tablo1 `UNION SELECT` sütun_adı `FROM` tablo2;
* `UNION` işlemcisi sadece farklı değerleri seçer, `UNION ALL` çoğaltmaya izin verir

### **INTERSECT**: İki SELECT ifadesinin ortak olan kayıtlarını döndürmek için kullanılan bir küme işlemcisidir.
* Genellikle yukarıda açıklandığı gibi **UNION** ile aynı şekilde kullanılır.
* `SELECT` sütun_isimleri `FROM` tablo1 `INTERSECT SELECT` sütun_isimleri `FROM` tablo2;

### **EXCEPT**: İkinci SELECT ifadesinde bulunmayan tüm kayıtları ilk SELECT ifadesinden döndürmek için kullanılan bir küme operatörüdür.
* Genellikle yukarıdaki **UNION** ile aynı şekilde kullanılır.
* `SELECT` sütun_adları `FROM` tablo1 `EXCEPT SELECT` sütun_adı `FROM` tablo2;

### **ANY|ALL**: WHERE veya HAVING ifadelerinde kullanılan alt sorgu koşullarını kontrol etmek için kullanılan bir operatördür.
* `ANY` operatörü, alt sorgu değerlerinden herhangi biri koşulu karşılarsa true değerini döndürür.
* `ALL` operatörü, tüm alt sorgu değerleri koşulu karşılarsa true değerini döndürür.
* `SELECT` sütun_adları `FROM` tablo1 `WHERE` sütun_adı operator (`ANY`|`ALL`) (`SELECT` sütun_adı `FROM` tablo_adı `WHERE` koşul);

### **GROUP BY**: Bir veya daha fazla sütuna göre sonuç kümesini gruplamak için sıklıkla toplama işlevleri (COUNT, MAX, MIN, SUM, AVG) ile kullanılan bir ifadedir.
* `SELECT` sütun_adı1, COUNT(sütun_adı2) `FROM` tablo_adı `WHERE` koşul `GROUP BY` sütun_adı1 `ORDER BY` COUNT(sütun_adı2) DESC;

### **HAVING**: bu koşul SQL'e agregasyon işlevleri ile WHERE anahtar kelimesinin kullanılamamasından dolayı eklenmiştir.
* `SELECT` `COUNT`(sütun_adı1), sütun_adı2 `FROM` tablo `GROUP BY` sütun_adı2 `HAVING` `COUNT(`sütun_adı1`)` > 5;

### **WITH**: Genellikle hiyerarşik verileri almak veya bir sorguda geçici sonuç kümesini birkaç kez yeniden kullanmak için kullanılır. Ayrıca "Ortak Tablo İfadesi" olarak da adlandırılır.
* `WITH RECURSIVE` oti `AS` (<br/>
    &nbsp;&nbsp;`SELECT` c0.* `FROM` kategoriler `AS` c0 `WHERE` id = 1 `# Başlangıç noktası`<br/>
    &nbsp;&nbsp;`UNION ALL`<br/>
    &nbsp;&nbsp;`SELECT` c1.* `FROM` kategoriler `AS` c1 `JOIN` cte `ON` c1.ana_kategori_id = oti.id<br/>
  )<br/>
  `SELECT` *<br/>
  `FROM` oti

<a name="degistir"></a>
# 2. Veri Değiştirme Sorguları

### **INSERT INTO**: Bir tabloya yeni kayıt/satır eklemek için kullanılır
* `INSERT INTO` tablo_adı (sütun1, sütun2) `VALUES` (değer1, değer2);
* `INSERT INTO` tablo_adı `VALUES` (değer1, değer2 …);

### **UPDATE**: Bir tablodaki mevcut kayıtları değiştirmek için kullanılır
* `UPDATE` tablo_adı `SET` sütun1 = değer1, sütun2 = değer2 `WHERE` koşul;
* `UPDATE` tablo_adı `SET` sütun_adı = değer;
  
###  Birden Fazla Tablodan Güncelleme
* `UPDATE` tablo_adi1 `SET` tablo_adi1.sutun1`=` tablo2.sutun1  `FROM` tablo2 `left join` tablo3  `ON` tablo2.id=tablo3.id `WHERE` tablo2.isim `IN` `(SELECT` isim `FROM` tablo3`)`


### **DELETE**: Bir tablodaki mevcut kayıtları/satırları silmek için kullanılır
* `DELETE FROM` tablo_adı `WHERE` koşul;
* `DELETE` * `FROM` tablo_adı;
* `TRUNCATE` tablo_adı;

<a name="rapor"></a>
# 3. Raporlama Sorguları

### **COUNT**: Belirtilen sütundaki kayıt sayısını döndürür
* `SELECT COUNT (DISTINCT` sütun_adı`)`;

### **MIN() ve MAX()**: Seçilen sütunun en küçük/en büyük değerini döndürür
* `SELECT MIN (`sütun_adları`) FROM` tablo_adı `WHERE` koşul;
* `SELECT MAX (`sütun_adları`) FROM` tablo_adı `WHERE` koşul;

### **AVG()**: Sayısal bir sütunun ortalama değerini döndürür
* `SELECT AVG (`sütun_adı`) FROM` tablo_adı `WHERE` koşul;

### **SUM()**: Sayısal bir sütunun toplamını döndürür
* `SELECT SUM (`sütun_adı`) FROM` tablo_adı `WHERE` koşul;

<a name="birlesim"></a>
# 4. Birleştirme Sorguları
### **INNER JOIN**: İki tabloda eşleşen değerleri döndürür
* `SELECT` sütun_isimleri `FROM` tablo1 `INNER JOIN` tablo2 `ON` tablo1.sütun_isim=tablo2.sütun_isim;
* `SELECT` tablo1.sütun_isim1, tablo2.sütun_isim2, tablo3.sütun_isim3 `FROM` ((tablo1 `INNER JOIN` tablo2 `ON` ilişki) `INNER JOIN` tablo3 `ON` ilişki);

### **LEFT (OUTER) JOIN**: Sol tablodan (tablo1) tüm kayıtları ve sağ tablodan (tablo2) eşleşen kayıtları döndürür
* `SELECT` sütun_isimleri `FROM` tablo1 `LEFT JOIN` tablo2 `ON` tablo1.sütun_isim=tablo2.sütun_isim;

### **RIGHT (OUTER) JOIN**: Sağ tablodan (tablo2) tüm kayıtları ve sol tablodan (tablo1) eşleşen kayıtları döndürür
* `SELECT` sütun_isimleri `FROM` tablo1 `RIGHT JOIN` tablo2 `ON` tablo1.sütun_isim=tablo2.sütun_isim;

### **FULL (OUTER) JOIN**: Her iki tabloda da eşleşme olduğunda tüm kayıtları döndürür
* `SELECT` sütun_isimleri `FROM` tablo1 ``FULL OUTER JOIN`` tablo2 `ON` tablo1.sütun_isim=tablo2.sütun_isim;

### **Self JOIN**: Bir tablo kendisiyle birleştirilir
* `SELECT` sütun_isimleri `FROM` tablo1 T1, tablo1 T2 `WHERE` koşul;

<a name="gorunum"></a>
# 5. Görünüm Sorguları

### **CREATE**: Bir görünüm oluşturur
* `CREATE VIEW` görünüm_adı `AS SELECT` sütun1, sütun2 `FROM` tablo_adı `WHERE` koşul;

### **SELECT**: Bir görünümü alır
* `SELECT` * `FROM` görünüm_adı;

### **DROP**: Bir görünümü siler
* `DROP VIEW` görünüm_adı;

<a name="degistirme"></a>
# 6. Tablo Değiştirme Sorguları

### **EKLEME**: Bir sütun ekleme
* `ALTER TABLE` tablo_adı `ADD` sütun_adı sütun_tanımı;

### **DEĞİŞTİRME**: Bir sütunun veri tipini değiştirme
* `ALTER TABLE` tablo_adı `MODIFY` sütun_adı sütun_tipi;

### **SİLME**: Bir sütunu silme
* `ALTER TABLE` tablo_adı `DROP COLUMN` sütun_adı;

<a name="olustur"></a>
# 7. Tablo Oluşturma Sorgusu

### **OLUŞTURMA**: Bir tablo oluşturma
* `CREATE TABLE` tablo_adı `(` <br />
   `sütun1` `veri_tipi`, <br />
   `sütun2` `veri_tipi`, <br />
   `sütun3` `veri_tipi`, <br />
   `sütun4` `veri_tipi`, <br />
   `);`

<a name="yorum"></a>
# 8. Sorgu Yorumları

### **TEK SATIR**: `--` ile başlar.
* `--DELETE * FROM tablo_adi;`

### **ÇOK SATIRLI**: `/*` ile başlar, `*/` ile biter.
* ```
  /*  Çok
      Satırlı
      Kesintiler */
      SELECT * FROM tablo_adı;
  ```

<a name="tetikleyici"></a>
# 8. Tetikleyici Oluşturma
Bir ekleme, güncelleme veya silme işleminden önce veya sonra otomatik olarak çalışan bir kod bloğudur. `old` ve `new` anahtar kelimeleri, tetikleyicinin etkilendiği satırlardaki sütunlara erişmemizi sağlar. Örnek: `new`.sütun_adi

### **Tetikleyici Oluşturma**:
 `delimiter` $\$<br />
`create trigger` tetikleyici_adi<br />
 <span>&nbsp;&nbsp;&nbsp;&nbsp;</span>`after | before` `insert | delete | update` `on` tablo_adi<br />
 <span>&nbsp;&nbsp;&nbsp;&nbsp;</span>`for each row`<br />
`begin`<br />
<span>&nbsp;&nbsp;&nbsp;&nbsp;</span> `------sql_sorgusu -------`<br />
`end ` $\$<br />
`delimiter` ;<br />

### **Tetikleyicileri Göster**: 
* `show triggers`;
* `show triggers` `like` %aranan_metin%'; 

### **Tetikleyiciyi Sil**:
* `drop trigger if exists` tetikleyici_adi;

<a name="prosedür"></a>
# 9. Saklı Prosedür Oluşturma
Saklı prosedürler SQL kodumuzu depolar ve düzenler, daha hızlı yürütme ve veri güvenliği sağlar.

### **Prosedür Oluşturma**:
 `delimiter` $\$<br />
`create procedure` prosedür_adi`()`<br />
`begin`<br />
<span>&nbsp;&nbsp;&nbsp;&nbsp;</span> `------sql_sorgusu -------`<br />
`end ` $\$<br />
`delimiter` ;<br />

### **Prosedürü Çağırma**: 
* `call` prosedür_adi`()`;

### **Prosedürü Silme**:
* `drop procedure if exists` prosedür_adi;

<a name="index"></a>
# 7. Sorgu İndexleme

### **OLUŞTURMA**: Bir tabloda indeks oluşturur.
* `CREATE INDEX` indeks_adi `ON` tablo_adi;
* `CREATE INDEX` indeks_adi `ON` tablo_adi (sutun_adi);
* `CREATE UNIQUE INDEX` indeks_adi `ON` tablo_adi (sutun_adi);
* `CREATE INDEX` indeks_adi `ON` tablo_adi (sutun1, sutun2);

### **SİLME**: Bir tablodan indeksi siler.
* `DROP INDEX` indeks_adi;
