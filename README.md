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

CALL getBolumbyn("M??hendislik, Mimarl??k ve Tasar??m Fak??ltesi");

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

CALL getDersbyN("Bilgisayar M??hendisli??i",". S??n??f", "G??z");
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


CALL getNot("Kal??t??m");

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

CALL setFakulte("Orman Fak??ltesi");
CALL setFakulte("Edebiyat Fak??ltesi");
CALL setFakulte("E??itim Fak??ltesi");
CALL setFakulte("Fen Fak??ltesi");
CALL setFakulte("??ktisadi ve ??dari Bilimler Fak??ltesi");
CALL setFakulte("??slami ??limler Fak??ltesi");
CALL setFakulte("M??hendislik, Mimarl??k ve Tasar??m Fak??ltesi");
CALL setFakulte("Sa??l??k Bilimleri Fak??ltesi");
CALL setFakulte("Spor Bilimleri Fak??ltesi");

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
CALL setBolum("Orman End??stri M??hendisli??i",1);
CALL setBolum("Orman M??hendisli??i",1);
CALL setBolum("Bilgi ve Belge Y??netimi",2);
CALL setBolum("??a??da?? T??rk Leh??eleri ve Edebiyatlar??",2);
CALL setBolum("Felsefe",2);
CALL setBolum("??ngilizce M??tercim ve Terc??manl??k (??ngilizce)",2);
CALL setBolum("Psikoloji",2);
CALL setBolum("Sanat Tarihi",2);
CALL setBolum("Sosyoloji",2);
CALL setBolum("Tarih",2);
CALL setBolum("T??rk Dili ve Edebiyat??",2);
CALL setBolum("Fen Bilgisi ????retmenli??i",3);
CALL setBolum("??lk????retim Matematik ????retmenli??i",3);
CALL setBolum("??ngilizce ????retmenli??i (??ngilizce)",3);
CALL setBolum("Rehberlik ve Psikolojik Dan????manl??k",3);
CALL setBolum("S??n??f ????retmenli??i",3);
CALL setBolum("Sosyal Bilgiler ????retmenli??i",3);
CALL setBolum("T??rk??e ????retmenli??i",3);
CALL setBolum("Resim-???? ????retmenli??i",3);
CALL setBolum("Bilgisayar Teknolojisi ve Bili??im Sistemleri",4);
CALL setBolum("Biyoteknoloji",4);
CALL setBolum("Matematik",4);
CALL setBolum("Molek??ler Biyoloji ve Genetik",4);
CALL setBolum("??ktisat",5);
CALL setBolum("????letme",5);
CALL setBolum("Siyaset Bilimi ve Kamu Y??netimi",5);
CALL setBolum("Uluslararas?? Ticaret ve Lojistik",5);
CALL setBolum("Y??netim Bili??im Sistemleri",5);
CALL setBolum("??slami ??limler",6);
CALL setBolum("Bilgisayar M??hendisli??i",7);
CALL setBolum("Elektrik-Elektronik M??hendisli??i",7);
CALL setBolum("Makine M??hendisli??i",7);
CALL setBolum("Peyzaj Mimarl??????",7);
CALL setBolum("Hem??irelik",8);
CALL setBolum("Sosyal Hizmet",8);
CALL setBolum("Antren??rl??k E??itimi",9);
CALL setBolum("Beden E??itimi ve Spor ????retmenli??i",9);
CALL setBolum("Rekreasyon",9);
CALL setBolum("Spor Y??neticili??i",9);

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


CALL setSinif("1. S??n??f");
CALL setSinif("2. S??n??f");
CALL setSinif("3. S??n??f");
CALL setSinif("4. S??n??f");

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

CALL setDonem("G??z");
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

CALL setDers("MAT1",7,30,1,1); /* fakulte, bolum , s??n??f, donem */
CALL setDers("F??Z??K1",7,30,1,1);
CALL setDers("B??YOLOJ??1",7,30,1,1);
CALL setDers("Algoritmalar ve Programlama",7,30,1,1);
CALL setDers("MAT2",7,30,1,2);
CALL setDers("F??Z??K2",7,30,1,2);
CALL setDers("Bilgisayar M??h. Giri??",7,30,1,2);
CALL setDers("Algoritmalar ve Programlama II",7,30,1,2);

CALL setDers("Ayr??k Matematik",7,30,2,1);
CALL setDers("Elektrik Devre Temelleri",7,30,2,1);
CALL setDers("Veri Yap??lar??",7,30,2,1);
CALL setDers("Lineer Cebir",7,30,2,1);
CALL setDers("Olas??l??k ve ??statistik",7,30,2,1); /* fakulte, bolum , s??n??f, donem */
CALL setDers("Nesneye Dayal?? Programlama",7,30,2,1);

CALL setDers("Say??sal ????z??mleme",7,30,2,2);
CALL setDers("Mant??ksal Devre Tasar??m??",7,30,2,2);
CALL setDers("Programlama Dilleri",7,30,2,2);
CALL setDers("Elektronik Devreler",7,30,2,2);
CALL setDers("Diferansiyel Denklemler",7,30,2,2);
CALL setDers("??nternet Tabanl?? Programlama",7,30,2,2);

CALL setDers("Veritaban?? Y??netim Sistemleri",7,30,3,1);
CALL setDers("Bilgisayar Mimarisi ve Organizasyonu",7,30,3,1);
CALL setDers("Veri Madencili??ine Giri??",7,30,3,1);
CALL setDers("Bilgisayar A??lar??",7,30,3,1);
CALL setDers("Sinyaller ve Sistemler",7,30,3,1);
CALL setDers(" Bi??imsel Diller ve Otomata",7,30,3,1);

CALL setDers("????letim Sistemleri",7,30,3,2);   /* fakulte, bolum , s??n??f, donem */
CALL setDers("Mesleki ??ngilizce",7,30,3,2);
CALL setDers("Sezgisel Algoritmalar",7,30,3,2);
CALL setDers("Web Tabanl?? Teknolojiler",7,30,3,2);
CALL setDers("Mikroi??lemciler",7,30,3,2);
CALL setDers("Yaz??l??m M??hendisli??i",7,30,3,2);

CALL setDers("G??r??nt?? ????lemenin Temelleri",7,30,4,1);
CALL setDers("Robotik Sistemlere Giri??",7,30,4,1);
CALL setDers("Makine ????renmesine Giri??",7,30,4,1);





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

CALL setKonu("T??rev","Sabit sayinin t??revi 0'd??r.",7,30,1,1,1); /* fakulte bolum s??n??f ders donem */
CALL setKonu("??ntegral", "Sabit say??lar??n integrali x dir.",7,30,1,1,1);
CALL setKonu("i??,g????,enerji","G??c??n form??l?? kuvvet*zaman d??r.",7,30,1,2,1);
CALL setKonu("Hareket","Hareketin form??l?? m*a d??r",7,30,1,2,1);
CALL setKonu("Kal??t??m","Kal??t??m soya??ekim veya irsiyet olarak da adland??r??l??r.",7,30,1,3,1);
CALL setKonu("??reme","??reme e??eyli ve e??eysiz olarak ikiye ayr??l??r",7,30,1,3,1);
CALL setKonu("Algoritma","Algoritma, belli bir problemi ????zmek veya belirli bir amaca ula??mak i??in tasarlanan yol",7,30,1,4,1);

CALL setKonu("Ters Simetrik","T??m a,b E A i??in sadece ve sadece a R b ve b R a, a=b anlam??na geliyorsa ters simetriktir.",7,30,1,1,2);
CALL setKonu("Ak??m","V=I*R Olarak g??sterilir.",7,30,1,2,2);
CALL setKonu("A??aca veri ekleme,silme","Ekleme ve silme i??lemleri a??ac??n en sonundan yap??l??r",7,30,1,3,2);
CALL setKonu("Lineer e??itlikler sistemi","n de??i??kenli iki veya daha fazla lineer e??itlikten olu??an bir sonlu k??meye lineer e??itlikler sistemi denir.",7,30,1,4,2);
CALL setKonu("??statistik","Genel anlam??yla istatistik , ya??ama ili??kin d??????ncelerin sistematize edilmi?? bi??imidir.",7,30,1,5,2);
CALL setKonu("Yaz??l??m dilleri","C#, pyhton, java birer yaz??l??m dilidir",7,30,1,6,2);


CALL setKonu("max (x)","x dizisinin en b??y??k eleman??n?? bulur. ",7,30,2,1,1);  /* fakulte bolum s??n??f ders donem */
CALL setKonu("2???ye t??mleme i??lemi","2???ye t??mleme i??lemi bir say??n??n i??aretini de??i??tirir. ",7,30,2,2,1);  
CALL setKonu("Atama ??fadeleri","??mperative dillerin ana hesaplama metodudur.",7,30,2,3,1);
CALL setKonu("Elektronik Devre","diren??, diyot, transist??r, kondansat??r ve ind??kt??r gibi devre elemanlar??n??n birbirlerine ba??lanmas??yla olu??turulan d??zeneklerdir.",7,30,2,4,1);
CALL setKonu("Derece","En y??ksek mertebeden t??revin derecesidir",7,30,2,5,1);
CALL setKonu("??nternet","B??t??n d??nyada kullan??lan, bilgisayar ve di??er ak??ll?? cihazlar arac??l??????yla veri ve bilgi iletmeyi/almay?? sa??layan ileti??im a????d??r.",7,30,2,6,1);


CALL setKonu("Tablo","Bir veri taban?? tablolarda saklanan verilerden olu??ur.",7,30,1,1,2); /* fakulte bolum s??n??f ders donem */
CALL setKonu("Von Neumann mimarisi","Komutlar ve veriler ayn?? bellekte yer al??r",7,30,1,2,2); 
CALL setKonu("Veri Temizleme","G??r??lt??l?? ve tutars??z verileri ????karmak",7,30,1,3,2); 
CALL setKonu("BlockHain","Bir kripto para birimi finansal hizmetler ??irketidir. ",7,30,1,4,2); 
CALL setKonu("Sinyal","Fiziksel bir b??y??kl?????? yada de??i??imi temsil eder",7,30,1,5,2);
CALL setKonu("Turing Machine","FA ile PDA n??n birle??tirilmi?? hali.",7,30,1,6,2);


CALL setKonu("Koruma ve hata kotarma","??ok kullan??c??l?? sistemlerde kullan??c??lar??n birbirlerinin hatalar??ndan etkilenmemesi",7,30,3,1,1);
CALL setKonu("Vocabulary","Kelime da??arc??????, bir ki??inin dilindeki tan??d??k kelimeler k??mesidir. ",7,30,3,2,1);
CALL setKonu("Kombinatoryal Optimizasyon","Nesneler ile ilgili s??ralama say??s??, ??zel matematiksel form??lasyonlardanyararlan??larak hesaplan??r.",7,30,3,3,1);
CALL setKonu("Hosting hizmeti"," web sitelerinin web a???? ??zerinden eri??ilebilmesi amac??yla web sunucular?? taraf??ndan verilen bar??nd??rma hizmetidir.",7,30,3,4,1);
CALL setKonu("MikroDenetleyicier"," otomotiv,  t??p, end??stri, askeri ve uzay teknolojileri  gibi bir??ok alanda  yo??un bir ??ekilde  kullan??lmaktad??r. ",7,30,3,5,1);
CALL setKonu("Sistem Yaz??l??m??","Di??er programlara hizmet sunmak ??zere haz??rlanm???? programlar",7,30,3,6,1);

CALL setKonu("Transformlar","De??i??ik domainlere d??n??????m yap??larak g??r??nt?? i??leme i??lemidir",7,30,3,1,2);/* fakulte bolum s??n??f ders donem */ 
CALL setKonu("Robot","Yeniden programlanabilen, maddeleri, par??alar??, aletleri programlanm???? hareketler ile i??e g??re ta????yan veya i??leyen ??ok fonksiyonlu makinedir.",7,30,3,2,2);
CALL setKonu("Yapay Zeka","Bir bilgisayar??n veya bilgisayar kontrol??ndeki bir robotun??e??itli faaliyetleri zeki canl??lara benzer ??ekilde yerine getirme kabiliyetidir",7,30,3,3,2);















SELECT * FROM Fakulte;
SELECT * FROM Bolum;
SELECT * FROM Konu;
SELECT * FROM Ders;
