# proton

Deep space data compression with error correction.

You can use bitflip to test the bit errors in space caused by radiation.

# Build

## protonzip:

```
g++ -std=c++20 -O2 main.cpp -lzstd -o protonzip
```

## bitflip:

```
g++ -std=c++20 -O2 bitflip.cpp -o bitflip
```

# Usage

## Protonzip:

### Maks güvenlik (daha az sıkıştırma, daha fazla CPU eforu, daha fazla tamir olasılığı)

```
./protonzip c dosya.ext output.pz -l 19 -b 1048576 -p 128 -i 128 -v
```

### Dengeli (sıkıştırma, CPU ve güvenlik dengesi)

```
./protonzip c dosya.ext output.pz -l 9 -b 8388608 -p 16 -i 32 -v
```

### Burst error korumalı

```
./protonzip c dosya.ext output.pz -l 15 -b 2097152 -p 32 -i 255 -v
```

## Geri açma

```
./protonzip d output.pz -v
```

## Base

```
./protonzip c [GİRDİ] [ÇIKTI] -l [SEVİYE] -b [BLOK] -p [PARITY] -i [DERİNLİK]
```

### Parametrelerin kısa özeti

- `-l` (Level): Zstd sıkıştırma gücü (1-19). Artarsa dosya küçülür ama işlem uzar.
- `-b` (Block Size): Verinin bölündüğü parçalar. Büyük bloklar daha iyi sıkıştırır.
- `-p` (Parity): Hata düzeltme kapasitesi. Ne kadar yüksekse o kadar çok hasar tamir edilir.
- `-i` (Interleave): Hataları dağıtma derinliği. Ardışık (yanyana) hataları engellemek için yükseltilmelidir.

### Önerilen senaryolar

| Senaryo             | Önerilen Komut            | Koruma Seviyesi | Hata Toleransı              |
| ------------------- | ------------------------- | --------------- | --------------------------- |
| Standart Arşiv      | -l 12 -b 1M -p 16 -i 8    | Düşük           | Küçük bit hataları          |
| Güvenli Depolama    | -l 15 -b 4M -p 32 -i 32   | Orta            | Disk bozulmaları (Bit-rot)  |
| Derin Uzay (Zırhlı) | -l 19 -b 1M -p 64 -i 64   | Yüksek          | Radyasyon / SEU hataları    |
| Voyager (Ekstrem)   | -l 19 -b 1M -p 128 -i 128 | Maksimum        | Fiziksel medya parçalanması |
