# not-sistemi
CREATE DATABASE Universite;
USE Universite;

/*
Tablolar
*/
CREATE TABLE Fakulte (
    FID INT AUTO_INCREMENT NOT NULL,
    FAd VARCHAR(255) NOT NULL,
   
    PRIMARY KEY (FID)
);

CREATE TABLE Bolum (
    BID INT AUTO_INCREMENT NOT NULL,
    BAd VARCHAR(255) NOT NULL,
    FID INT  NOT NULL,
	
    PRIMARY KEY (BID),
    FOREIGN KEY (FID) REFERENCES Fakulte(FID)
);

CREATE TABLE Sinif (
    SID INT AUTO_INCREMENT NOT NULL,
    SAd VARCHAR(255) NOT NULL,
   
   PRIMARY KEY (SID)
);

CREATE TABLE Donem (
    DoID INT AUTO_INCREMENT NOT NULL,
    DoAd VARCHAR(255) NOT NULL,
   
   PRIMARY KEY (DoID)
);

CREATE TABLE Ders (
    DID INT AUTO_INCREMENT NOT NULL,
    DAd VARCHAR(255) NOT NULL,
    
    SID INT  NOT NULL,
    DoID INT NOT NULL,
    BID INT NOT NULL,
    FID INT NOT NULL,

	PRIMARY KEY (DID),
	FOREIGN KEY (FID) REFERENCES Fakulte(FID),
    FOREIGN KEY (BID) REFERENCES Bolum(BID),
    FOREIGN KEY (SID) REFERENCES Sinif(SID),
    FOREIGN KEY (DoID) REFERENCES Donem(DoID)
);

CREATE TABLE Konu (
    KID INT AUTO_INCREMENT NOT NULL,
    KAd VARCHAR(255) NOT NULL,
    KNot VARCHAR(255) NOT NULL,
    
    SID INT  NOT NULL,
	DoID INT NOT NULL,
    BID INT NOT NULL,
    FID INT NOT NULL,
	DID INT  NOT NULL,
    
	PRIMARY KEY (KID),
	FOREIGN KEY (FID) REFERENCES Fakulte(FID),
    FOREIGN KEY (BID) REFERENCES Bolum(BID),
    FOREIGN KEY (SID) REFERENCES Sinif(SID),
    FOREIGN KEY (DoID) REFERENCES Donem(DoID),
	FOREIGN KEY (DID) REFERENCES Ders(DID)
);

/*
PROCEDURE veya Fonksiyonlar
*/

/*Fakulte almak icin*/
DELIMITER $$
CREATE PROCEDURE getFakulte()
BEGIN
SELECT Fakulte.FAd FROM Fakulte;
END $$
DELIMITER ;

/*CALL ile fonksiyonu veya procedure calistiryoruz.*/
CALL getFakulte();

/*Bolum almak icin*/
DELIMITER $$
CREATE PROCEDURE getBolum()
BEGIN
SELECT Bolum.BAd FROM Bolum;
END $$
DELIMITER ;

CALL getBolum();

DELIMITER $$
CREATE PROCEDURE getBolumbyN(
		IN fad VARCHAR(255)
)
BEGIN
SELECT Bolum.BAd FROM Bolum
WHERE Bolum.FID IN (SELECT Fakulte.FID FROM Fakulte WHERE Fakulte.FAd = fad);
END $$
DELIMITER ;

CALL getBolumbyn("Mühendislik, Mimarlık ve Tasarım Fakültesi");

/*Sinif almak icin*/
DELIMITER $$
CREATE PROCEDURE getSinif()
BEGIN
SELECT Sinif.SAd FROM Sinif;
END $$
DELIMITER ;

CALL getSinif();

/*Donem almak icin*/
DELIMITER $$
CREATE PROCEDURE getDonem()
BEGIN
SELECT Donem.DoAd FROM Donem;
END $$
DELIMITER ;

CALL getDonem();

/*Ders almak icin*/
DELIMITER $$
CREATE PROCEDURE getDers()
BEGIN
SELECT Ders.DAd FROM Ders;
END $$
DELIMITER ;

CALL getDers();

DELIMITER $$
CREATE PROCEDURE getDersbyN(
	IN bad VARCHAR(255), IN sad VARCHAR(255), IN doad VARCHAR(255)
)
BEGIN
SELECT DISTINCT Ders.DAd 
FROM Ders,Sinif,Bolum,Donem,Fakulte
WHERE Ders.BID IN (SELECT Bolum.BID FROM Bolum WHERE Bolum.BAd = bad) AND 
Ders.SID IN (SELECT Sinif.SID FROM Sinif WHERE Sinif.SAd = sad) AND
Ders.DoID IN (SELECT Donem.DoID FROM Donem WHERE Donem.DoAd = doad);
END $$
DELIMITER ;

CALL getDersbyN("Bilgisayar Mühendisliği",". Sınıf", "Güz");
/*Konu almak icin*/
DELIMITER $$
CREATE PROCEDURE getKonu()
BEGIN
SELECT Konu.KAd FROM Konu;
END $$
DELIMITER ;



CALL getKonu();

/*Notlari almak icin*/
DELIMITER $$
CREATE PROCEDURE getNot(
	IN konuad VARCHAR(255)
)
BEGIN
SELECT Konu.KNot 
FROM Konu
WHERE Konu.KAd = konuad;
END $$
DELIMITER ;


CALL getNot("Kalıtım");

/*Fakulte eklemek icin*/
DELIMITER $$
CREATE PROCEDURE setFakulte(
	IN fad VARCHAR(255)
)
BEGIN
INSERT INTO Fakulte (FAd)
VALUES (fad);
END $$
DELIMITER ;

CALL setFakulte("Orman Fakültesi");
CALL setFakulte("Edebiyat Fakültesi");
CALL setFakulte("Eğitim Fakültesi");
CALL setFakulte("Fen Fakültesi");
CALL setFakulte("İktisadi ve İdari Bilimler Fakültesi");
CALL setFakulte("İslami İlimler Fakültesi");
CALL setFakulte("Mühendislik, Mimarlık ve Tasarım Fakültesi");
CALL setFakulte("Sağlık Bilimleri Fakültesi");
CALL setFakulte("Spor Bilimleri Fakültesi");

/*Bolum eklemek icin*/
DELIMITER $$
CREATE PROCEDURE setBolum(
	IN bad VARCHAR(255), IN fid INT
)
BEGIN
INSERT INTO Bolum (BAd,FID)
VALUES (bad,fid);
END $$
DELIMITER ;

SELECT * FROM Fakulte;
CALL setBolum("Orman Endüstri Mühendisliği",1);
CALL setBolum("Orman Mühendisliği",1);
CALL setBolum("Bilgi ve Belge Yönetimi",2);
CALL setBolum("Çağdaş Türk Lehçeleri ve Edebiyatları",2);
CALL setBolum("Felsefe",2);
CALL setBolum("İngilizce Mütercim ve Tercümanlık (İngilizce)",2);
CALL setBolum("Psikoloji",2);
CALL setBolum("Sanat Tarihi",2);
CALL setBolum("Sosyoloji",2);
CALL setBolum("Tarih",2);
CALL setBolum("Türk Dili ve Edebiyatı",2);
CALL setBolum("Fen Bilgisi Öğretmenliği",3);
CALL setBolum("İlköğretim Matematik Öğretmenliği",3);
CALL setBolum("İngilizce Öğretmenliği (İngilizce)",3);
CALL setBolum("Rehberlik ve Psikolojik Danışmanlık",3);
CALL setBolum("Sınıf Öğretmenliği",3);
CALL setBolum("Sosyal Bilgiler Öğretmenliği",3);
CALL setBolum("Türkçe Öğretmenliği",3);
CALL setBolum("Resim-İş Öğretmenliği",3);
CALL setBolum("Bilgisayar Teknolojisi ve Bilişim Sistemleri",4);
CALL setBolum("Biyoteknoloji",4);
CALL setBolum("Matematik",4);
CALL setBolum("Moleküler Biyoloji ve Genetik",4);
CALL setBolum("İktisat",5);
CALL setBolum("İşletme",5);
CALL setBolum("Siyaset Bilimi ve Kamu Yönetimi",5);
CALL setBolum("Uluslararası Ticaret ve Lojistik",5);
CALL setBolum("Yönetim Bilişim Sistemleri",5);
CALL setBolum("İslami İlimler",6);
CALL setBolum("Bilgisayar Mühendisliği",7);
CALL setBolum("Elektrik-Elektronik Mühendisliği",7);
CALL setBolum("Makine Mühendisliği",7);
CALL setBolum("Peyzaj Mimarlığı",7);
CALL setBolum("Hemşirelik",8);
CALL setBolum("Sosyal Hizmet",8);
CALL setBolum("Antrenörlük Eğitimi",9);
CALL setBolum("Beden Eğitimi ve Spor Öğretmenliği",9);
CALL setBolum("Rekreasyon",9);
CALL setBolum("Spor Yöneticiliği",9);

/*Sinif eklemek icin*/
DELIMITER $$
CREATE PROCEDURE setSinif(
	IN sad VARCHAR(255)
)
BEGIN
INSERT INTO Sinif (SAd)
VALUES (sad);
END $$
DELIMITER ;


CALL setSinif("1. Sınıf");
CALL setSinif("2. Sınıf");
CALL setSinif("3. Sınıf");
CALL setSinif("4. Sınıf");

/*Donem eklemek icin*/
DELIMITER $$
CREATE PROCEDURE setDonem(
	IN doad VARCHAR(255)
)
BEGIN
INSERT INTO Donem (DoAd)
VALUES (doad);
END $$
DELIMITER ;

CALL setDonem("Güz");
CALL setDonem("Bahar");

/*Ders eklemek icin*/
DELIMITER $$
CREATE PROCEDURE setDers(
	IN dad VARCHAR(255), fid INT,  bid INT, sid INT, doid INT 
)
BEGIN
INSERT INTO Ders (DAd,FID,BID,SID,DoID)
VALUES (dad,fid,bid,sid,doid);
END $$
DELIMITER ;

CALL setDers("MAT1",7,30,1,1); /* fakulte, bolum , sınıf, donem */
CALL setDers("FİZİK1",7,30,1,1);
CALL setDers("BİYOLOJİ1",7,30,1,1);
CALL setDers("Algoritmalar ve Programlama",7,30,1,1);
CALL setDers("MAT2",7,30,1,2);
CALL setDers("FİZİK2",7,30,1,2);
CALL setDers("Bilgisayar Müh. Giriş",7,30,1,2);
CALL setDers("Algoritmalar ve Programlama II",7,30,1,2);

CALL setDers("Ayrık Matematik",7,30,2,1);
CALL setDers("Elektrik Devre Temelleri",7,30,2,1);
CALL setDers("Veri Yapıları",7,30,2,1);
CALL setDers("Lineer Cebir",7,30,2,1);
CALL setDers("Olasılık ve İstatistik",7,30,2,1); /* fakulte, bolum , sınıf, donem */
CALL setDers("Nesneye Dayalı Programlama",7,30,2,1);

CALL setDers("Sayısal Çözümleme",7,30,2,2);
CALL setDers("Mantıksal Devre Tasarımı",7,30,2,2);
CALL setDers("Programlama Dilleri",7,30,2,2);
CALL setDers("Elektronik Devreler",7,30,2,2);
CALL setDers("Diferansiyel Denklemler",7,30,2,2);
CALL setDers("İnternet Tabanlı Programlama",7,30,2,2);

CALL setDers("Veritabanı Yönetim Sistemleri",7,30,3,1);
CALL setDers("Bilgisayar Mimarisi ve Organizasyonu",7,30,3,1);
CALL setDers("Veri Madenciliğine Giriş",7,30,3,1);
CALL setDers("Bilgisayar Ağları",7,30,3,1);
CALL setDers("Sinyaller ve Sistemler",7,30,3,1);
CALL setDers(" Biçimsel Diller ve Otomata",7,30,3,1);

CALL setDers("İşletim Sistemleri",7,30,3,2);   /* fakulte, bolum , sınıf, donem */
CALL setDers("Mesleki İngilizce",7,30,3,2);
CALL setDers("Sezgisel Algoritmalar",7,30,3,2);
CALL setDers("Web Tabanlı Teknolojiler",7,30,3,2);
CALL setDers("Mikroişlemciler",7,30,3,2);
CALL setDers("Yazılım Mühendisliği",7,30,3,2);

CALL setDers("Görüntü İşlemenin Temelleri",7,30,4,1);
CALL setDers("Robotik Sistemlere Giriş",7,30,4,1);
CALL setDers("Makine Öğrenmesine Giriş",7,30,4,1);





/*Konu eklemek icin*/
DELIMITER $$
CREATE PROCEDURE setKonu(
	IN kad VARCHAR(255), knot VARCHAR(255), fid INT,  bid INT, sid INT , did INT,
    doid INT
)
BEGIN
INSERT INTO Konu (KAd,KNot,FID,BID,SID,DID,DoID)
VALUES (kad,knot,fid,bid,sid,did,doid);
END $$
DELIMITER ;

CALL setKonu("Türev","Sabit sayinin türevi 0'dır.",7,30,1,1,1); /* fakulte bolum sınıf ders donem */
CALL setKonu("İntegral", "Sabit sayıların integrali x dir.",7,30,1,1,1);
CALL setKonu("iş,güç,enerji","Gücün formülü kuvvet*zaman dır.",7,30,1,2,1);
CALL setKonu("Hareket","Hareketin formülü m*a dır",7,30,1,2,1);
CALL setKonu("Kalıtım","Kalıtım soyaçekim veya irsiyet olarak da adlandırılır.",7,30,1,3,1);
CALL setKonu("Üreme","Üreme eşeyli ve eşeysiz olarak ikiye ayrılır",7,30,1,3,1);
CALL setKonu("Algoritma","Algoritma, belli bir problemi çözmek veya belirli bir amaca ulaşmak için tasarlanan yol",7,30,1,4,1);

CALL setKonu("Ters Simetrik","Tüm a,b E A için sadece ve sadece a R b ve b R a, a=b anlamına geliyorsa ters simetriktir.",7,30,1,1,2);
CALL setKonu("Akım","V=I*R Olarak gösterilir.",7,30,1,2,2);
CALL setKonu("Ağaca veri ekleme,silme","Ekleme ve silme işlemleri ağacın en sonundan yapılır",7,30,1,3,2);
CALL setKonu("Lineer eşitlikler sistemi","n değişkenli iki veya daha fazla lineer eşitlikten oluşan bir sonlu kümeye lineer eşitlikler sistemi denir.",7,30,1,4,2);
CALL setKonu("İstatistik","Genel anlamıyla istatistik , yaşama ilişkin düşüncelerin sistematize edilmiş biçimidir.",7,30,1,5,2);
CALL setKonu("Yazılım dilleri","C#, pyhton, java birer yazılım dilidir",7,30,1,6,2);


CALL setKonu("max (x)","x dizisinin en büyük elemanını bulur. ",7,30,2,1,1);  /* fakulte bolum sınıf ders donem */
CALL setKonu("2’ye tümleme işlemi","2’ye tümleme işlemi bir sayının işaretini değiştirir. ",7,30,2,2,1);  
CALL setKonu("Atama İfadeleri","İmperative dillerin ana hesaplama metodudur.",7,30,2,3,1);
CALL setKonu("Elektronik Devre","direnç, diyot, transistör, kondansatör ve indüktör gibi devre elemanlarının birbirlerine bağlanmasıyla oluşturulan düzeneklerdir.",7,30,2,4,1);
CALL setKonu("Derece","En yüksek mertebeden türevin derecesidir",7,30,2,5,1);
CALL setKonu("İnternet","Bütün dünyada kullanılan, bilgisayar ve diğer akıllı cihazlar aracılığıyla veri ve bilgi iletmeyi/almayı sağlayan iletişim ağıdır.",7,30,2,6,1);


CALL setKonu("Tablo","Bir veri tabanı tablolarda saklanan verilerden oluşur.",7,30,1,1,2); /* fakulte bolum sınıf ders donem */
CALL setKonu("Von Neumann mimarisi","Komutlar ve veriler aynı bellekte yer alır",7,30,1,2,2); 
CALL setKonu("Veri Temizleme","Gürültülü ve tutarsız verileri çıkarmak",7,30,1,3,2); 
CALL setKonu("BlockHain","Bir kripto para birimi finansal hizmetler şirketidir. ",7,30,1,4,2); 
CALL setKonu("Sinyal","Fiziksel bir büyüklüğü yada değişimi temsil eder",7,30,1,5,2);
CALL setKonu("Turing Machine","FA ile PDA nın birleştirilmiş hali.",7,30,1,6,2);


CALL setKonu("Koruma ve hata kotarma","çok kullanıcılı sistemlerde kullanıcıların birbirlerinin hatalarından etkilenmemesi",7,30,3,1,1);
CALL setKonu("Vocabulary","Kelime dağarcığı, bir kişinin dilindeki tanıdık kelimeler kümesidir. ",7,30,3,2,1);
CALL setKonu("Kombinatoryal Optimizasyon","Nesneler ile ilgili sıralama sayısı, özel matematiksel formülasyonlardanyararlanılarak hesaplanır.",7,30,3,3,1);
CALL setKonu("Hosting hizmeti"," web sitelerinin web ağı üzerinden erişilebilmesi amacıyla web sunucuları tarafından verilen barındırma hizmetidir.",7,30,3,4,1);
CALL setKonu("MikroDenetleyicier"," otomotiv,  tıp, endüstri, askeri ve uzay teknolojileri  gibi birçok alanda  yoğun bir şekilde  kullanılmaktadır. ",7,30,3,5,1);
CALL setKonu("Sistem Yazılımı","Diğer programlara hizmet sunmak üzere hazırlanmış programlar",7,30,3,6,1);

CALL setKonu("Transformlar","Değişik domainlere dönüşüm yapılarak görüntü işleme işlemidir",7,30,3,1,2);/* fakulte bolum sınıf ders donem */ 
CALL setKonu("Robot","Yeniden programlanabilen, maddeleri, parçaları, aletleri programlanmış hareketler ile işe göre taşıyan veya işleyen çok fonksiyonlu makinedir.",7,30,3,2,2);
CALL setKonu("Yapay Zeka","Bir bilgisayarın veya bilgisayar kontrolündeki bir robotunçeşitli faaliyetleri zeki canlılara benzer şekilde yerine getirme kabiliyetidir",7,30,3,3,2);















SELECT * FROM Fakulte;
SELECT * FROM Bolum;
SELECT * FROM Konu;
SELECT * FROM Ders;
