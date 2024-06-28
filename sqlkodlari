CREATE DATABASE RestoranYonetim;
GO
USE RestoranYonetim;
GO
CREATE TABLE Musteriler (
    MusteriID VARCHAR(64) NOT NULL PRIMARY KEY,
    Ad VARCHAR(64) NOT NULL,
    Soyad VARCHAR(64) NOT NULL,
    Telefon VARCHAR(25) NOT NULL,
    Mail VARCHAR(250) NOT NULL,
    Adres VARCHAR(250) NOT NULL
);

CREATE TABLE MenuOgeleri (
    UrunID VARCHAR(64) NOT NULL PRIMARY KEY,
    UrunAd VARCHAR(250) NOT NULL,
    Kategori VARCHAR(250) NOT NULL,
    BirimFiyat FLOAT NOT NULL,
    StokMiktari FLOAT NOT NULL,
    Birim VARCHAR(16) NOT NULL,
    Detay VARCHAR(250) NOT NULL
);

CREATE TABLE Siparisler (
    SiparisID VARCHAR(64) NOT NULL PRIMARY KEY,
    MusteriID VARCHAR(64) NOT NULL,
    SiparisTarihi DATETIME NOT NULL,
    SiparisToplami FLOAT NOT NULL,
    FOREIGN KEY (MusteriID) REFERENCES Musteriler(MusteriID)
        ON DELETE CASCADE ON UPDATE CASCADE
);

CREATE TABLE SiparisDetaylari (
    SiparisID VARCHAR(64) NOT NULL,
    UrunID VARCHAR(64) NOT NULL,
    UrunAdedi INT NOT NULL,
    UrunFiyati FLOAT NOT NULL,
    PRIMARY KEY (SiparisID, UrunID),
    FOREIGN KEY (SiparisID) REFERENCES Siparisler(SiparisID)
        ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (UrunID) REFERENCES MenuOgeleri(UrunID)
        ON DELETE CASCADE ON UPDATE CASCADE
);

CREATE TABLE Odemeler (
    OdemeID VARCHAR(64) NOT NULL PRIMARY KEY,
    MusteriID VARCHAR(64) NOT NULL,
    OdemeTarihi DATETIME NOT NULL,
    OdemeTutari FLOAT NOT NULL,
    OdemeTuru VARCHAR(25) NOT NULL,
    Aciklama VARCHAR(250) NOT NULL,
    FOREIGN KEY (MusteriID) REFERENCES Musteriler(MusteriID)
        ON DELETE CASCADE ON UPDATE CASCADE
);

-- Müşteriler İçin Stored Procedures
CREATE PROCEDURE MusterilerHepsi
AS
BEGIN
    SELECT 
        MusteriID AS ID,
        Ad AS Adı,
        Soyad AS Soyadı,
        Telefon AS Telefon,
        Mail AS Mail,
        Adres AS Adres
    FROM Musteriler;
END;
GO

CREATE PROCEDURE MusteriEkle
    @id VARCHAR(64),
    @ad VARCHAR(64),
    @soyad VARCHAR(64),
    @tel VARCHAR(25),
    @mail VARCHAR(250),
    @adres VARCHAR(250)
AS
BEGIN
    INSERT INTO Musteriler (MusteriID, Ad, Soyad, Telefon, Mail, Adres)
    VALUES (@id, @ad, @soyad, @tel, @mail, @adres);
END;
GO

CREATE PROCEDURE MusteriGuncelle
    @id VARCHAR(64),
    @ad VARCHAR(64),
    @soyad VARCHAR(64),
    @tel VARCHAR(25),
    @mail VARCHAR(250),
    @adres VARCHAR(250)
AS
BEGIN
    UPDATE Musteriler
    SET 
        Ad = @ad,
        Soyad = @soyad,
        Telefon = @tel,
        Mail = @mail,
        Adres = @adres
    WHERE MusteriID = @id;
END;
GO

CREATE PROCEDURE MusteriSil
    @id VARCHAR(64)
AS
BEGIN
    DELETE FROM Musteriler
    WHERE MusteriID = @id;
END;
GO

CREATE PROCEDURE MusteriBul
    @filtre VARCHAR(32)
AS
BEGIN
    SELECT * FROM Musteriler
    WHERE 
        MusteriID LIKE '%' + @filtre + '%' OR
        Ad LIKE '%' + @filtre + '%' OR
        Soyad LIKE '%' + @filtre + '%' OR
        Telefon LIKE '%' + @filtre + '%' OR
        Mail LIKE '%' + @filtre + '%' OR
        Adres LIKE '%' + @filtre + '%';
END;
GO

CREATE PROCEDURE MusteriSiparisler
    @id VARCHAR(64)
AS
BEGIN
    SELECT * FROM Siparisler
    WHERE MusteriID = @id;
END;
GO

-- Menü Öğeleri İçin Stored Procedures
CREATE PROCEDURE MenuOgeEkle
    @id VARCHAR(64),
    @ad VARCHAR(250),
    @kategori VARCHAR(250),
    @fiyat FLOAT,
    @stok FLOAT,
    @birim VARCHAR(16),
    @detay VARCHAR(250)
AS
BEGIN
    INSERT INTO MenuOgeleri (UrunID, UrunAd, Kategori, BirimFiyat, StokMiktari, Birim, Detay)
    VALUES (@id, @ad, @kategori, @fiyat, @stok, @birim, @detay);
END;
GO

CREATE PROCEDURE MenuOgeGuncelle
    @id VARCHAR(64),
    @ad VARCHAR(250),
    @kategori VARCHAR(250),
    @fiyat FLOAT,
    @stok FLOAT,
    @birim VARCHAR(16),
    @detay VARCHAR(250)
AS
BEGIN
    UPDATE MenuOgeleri
    SET 
        UrunAd = @ad,
        Kategori = @kategori,
        BirimFiyat = @fiyat,
        StokMiktari = @stok,
        Birim = @birim,
        Detay = @detay
    WHERE UrunID = @id;
END;
GO

CREATE PROCEDURE MenuOgeStokGuncelle
    @id VARCHAR(64),
    @stok FLOAT
AS
BEGIN
    UPDATE MenuOgeleri
    SET 
        StokMiktari = @stok
    WHERE UrunID = @id;
END;
GO

CREATE PROCEDURE MenuOgeSil
    @id VARCHAR(64)
AS
BEGIN
    DELETE FROM MenuOgeleri
    WHERE UrunID = @id;
END;
GO

CREATE PROCEDURE MenuOgeBul
    @filtre VARCHAR(64)
AS
BEGIN
    SELECT * FROM MenuOgeleri
    WHERE 
        UrunID LIKE '%' + @filtre + '%' OR
        UrunAd LIKE '%' + @filtre + '%' OR
        Kategori LIKE '%' + @filtre + '%';
END;
GO

-- Siparişler İçin Stored Procedures
CREATE PROCEDURE SiparisEkle
    @id VARCHAR(64),
    @musteri_id VARCHAR(64),
    @tarih DATETIME,
    @toplam FLOAT
AS
BEGIN
    INSERT INTO Siparisler (SiparisID, MusteriID, SiparisTarihi, SiparisToplami)
    VALUES (@id, @musteri_id, @tarih, @toplam);
END;
GO

CREATE PROCEDURE SiparisDetayEkle
    @siparis_id VARCHAR(64),
    @urun_id VARCHAR(64),
    @adet INT,
    @fiyat FLOAT
AS
BEGIN
    INSERT INTO SiparisDetaylari (SiparisID, UrunID, UrunAdedi, UrunFiyati)
    VALUES (@siparis_id, @urun_id, @adet, @fiyat);
END;
GO

CREATE PROCEDURE SiparisToplamGuncelle
    @id VARCHAR(64),
    @toplam FLOAT
AS
BEGIN
    UPDATE Siparisler
    SET SiparisToplami = @toplam
    WHERE SiparisID = @id;
END;
GO

CREATE PROCEDURE SiparisDetayGuncelle
    @siparis_id VARCHAR(64),
    @urun_id VARCHAR(64),
    @adet INT,
    @fiyat FLOAT
AS
BEGIN
    UPDATE SiparisDetaylari
    SET 
        UrunAdedi = @adet,
        UrunFiyati = @fiyat
    WHERE SiparisID = @siparis_id AND UrunID = @urun_id;
END;
GO

CREATE PROCEDURE SiparisDetaySil
    @siparis_id VARCHAR(64),
    @urun_id VARCHAR(64)
AS
BEGIN
    DELETE FROM SiparisDetaylari
    WHERE SiparisID = @siparis_id AND UrunID = @urun_id;
END;
GO

CREATE PROCEDURE SiparisSil
    @id VARCHAR(64)
AS
BEGIN
    DELETE FROM Siparisler
    WHERE SiparisID = @id;
END;
GO

CREATE PROCEDURE MusteriSiparisDetay
    @id VARCHAR(64)
AS
BEGIN
    SELECT 
        S.SiparisID, 
        M.UrunID, 
        M.UrunAd, 
        M.Kategori, 
        D.UrunAdedi, 
        D.UrunFiyati, 
        (D.UrunAdedi * D.UrunFiyati) AS ToplamFiyat
    FROM Siparisler S
    JOIN SiparisDetaylari D ON S.SiparisID = D.SiparisID
    JOIN MenuOgeleri M ON D.UrunID = M.UrunID
    WHERE S.MusteriID = @id;
END;
GO

-- Ödemeler İçin Stored Procedures
CREATE PROCEDURE OdemeEkle
    @id VARCHAR(64),
    @musteri_id VARCHAR(64),
    @tarih DATETIME,
    @tutar FLOAT,
    @tur VARCHAR(25),
    @aciklama VARCHAR(250)
AS
BEGIN
    INSERT INTO Odemeler (OdemeID, MusteriID, OdemeTarihi, OdemeTutari, OdemeTuru, Aciklama)
    VALUES (@id, @musteri_id, @tarih, @tutar, @tur, @aciklama);
END;
GO

CREATE PROCEDURE OdemeGuncelle
    @id VARCHAR(64),
    @musteri_id VARCHAR(64),
    @tarih DATETIME,
    @tutar FLOAT,
    @tur VARCHAR(25),
    @aciklama VARCHAR(250)
AS
BEGIN
    UPDATE Odemeler
    SET 
        MusteriID = @musteri_id,
        OdemeTarihi = @tarih,
        OdemeTutari = @tutar,
        OdemeTuru = @tur,
        Aciklama = @aciklama
    WHERE OdemeID = @id;
END;
GO

CREATE PROCEDURE OdemeSil
    @id VARCHAR(64)
AS
BEGIN
    DELETE FROM Odemeler
    WHERE OdemeID = @id;
END;
GO

CREATE PROCEDURE MusteriOdemeler
    @id VARCHAR(64)
AS
BEGIN
    SELECT * FROM Odemeler
    WHERE MusteriID = @id;
END;
GO
-- Musteriler tablosuna yeni müşteri eklenirken tetiklenecek trigger
CREATE TRIGGER trg_MusteriEklemeLog
ON Musteriler
AFTER INSERT
AS
BEGIN
    -- Log tablosunun oluşturulması (eğer yoksa)
    IF NOT EXISTS (SELECT * FROM sysobjects WHERE name='MusteriLog' AND xtype='U')
    CREATE TABLE MusteriLog (
        LogID INT IDENTITY(1,1) PRIMARY KEY,
        MusteriID VARCHAR(64),
        Ad VARCHAR(64),
        Soyad VARCHAR(64),
        Telefon VARCHAR(25),
        Mail VARCHAR(250),
        Adres VARCHAR(250),
        EklenmeTarihi DATETIME
    );

    -- Yeni eklenen müşterinin log tablosuna kaydedilmesi
    INSERT INTO MusteriLog (MusteriID, Ad, Soyad, Telefon, Mail, Adres, EklenmeTarihi)
    SELECT MusteriID, Ad, Soyad, Telefon, Mail, Adres, GETDATE()
    FROM inserted;
END;
GO
-- MenuOgeleri tablosunda ürün güncellenirken tetiklenecek trigger
CREATE TRIGGER trg_MenuOgeGuncellemeLog
ON MenuOgeleri
AFTER UPDATE
AS
BEGIN
    -- Log tablosunun oluşturulması (eğer yoksa)
    IF NOT EXISTS (SELECT * FROM sysobjects WHERE name='MenuOgeLog' AND xtype='U')
    CREATE TABLE MenuOgeLog (
        LogID INT IDENTITY(1,1) PRIMARY KEY,
        UrunID VARCHAR(64),
        EskiUrunAd VARCHAR(250),
        YeniUrunAd VARCHAR(250),
        EskiKategori VARCHAR(250),
        YeniKategori VARCHAR(250),
        EskiFiyat FLOAT,
        YeniFiyat FLOAT,
        EskiStok FLOAT,
        YeniStok FLOAT,
        GuncellenmeTarihi DATETIME
    );

    -- Güncellenen ürünün log tablosuna kaydedilmesi
    INSERT INTO MenuOgeLog (UrunID, EskiUrunAd, YeniUrunAd, EskiKategori, YeniKategori, EskiFiyat, YeniFiyat, EskiStok, YeniStok, GuncellenmeTarihi)
    SELECT 
        d.UrunID,
        d.UrunAd AS EskiUrunAd,
        i.UrunAd AS YeniUrunAd,
        d.Kategori AS EskiKategori,
        i.Kategori AS YeniKategori,
        d.BirimFiyat AS EskiFiyat,
        i.BirimFiyat AS YeniFiyat,
        d.StokMiktari AS EskiStok,
        i.StokMiktari AS YeniStok,
        GETDATE() AS GuncellenmeTarihi
    FROM deleted d
    JOIN inserted i ON d.UrunID = i.UrunID;
END;
GO
