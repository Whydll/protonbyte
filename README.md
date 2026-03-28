# proton

Deep space data compression

You can use bitflip to test the bit errors in space coused by radiation

# build

protonzip:

```
g++ -std=c++20 -O2 main.cpp -lzstd -o protonzip
```

bitflip:
g++ ./bitflip.cpp -o bitflip

# Usage:

## Protonzip:

Maks güvenlik: (daha az sıkıştırma ve daha fazla cpu eforu, daha fazla tamir olasılığı için)

```
./protonzip c dosya.ext output.pz -l 19 -b 1048576 -p 128 -i 128 -v
```

Dengeli: (sıkıştırma, cpu ve güvenlik arasında dengeli mod)

```
./protonzip c dosya.ext output.pz -l 9 -b 8388608 -p 16 -i 32 -v
```

Burst error korumalı ( ne bilmitorum araştırıp yazmayı unutma )

```
./protonzip c dosya.ext output.pz -l 15 -b 2097152 -p 32 -i 255 -v
```

### Geri ayıklama

```
./protonzip d output.pz -v
```

### base:

```
./protonzip c dosya.ext output.pz -l -b -p -i
```

./protonzip c [GİRDİ] [ÇIKTI] -l [SEVİYE] -b [BLOK] -p [PARITY] -i [DERİNLİK]

### Parametrelerin Kısa Özeti:

-l (Level): Zstd sıkıştırma gücü (1-19). Artarsa dosya küçülür ama işlem uzar.

-b (Block Size): Verinin bölündüğü parçalar. Büyük bloklar daha iyi sıkıştırır.

-p (Parity): Hata düzeltme kapasitesi. Ne kadar yüksekse o kadar çok hasar tamir edilir.

-i (Interleave): Hataları dağıtma derinliği. Ardışık (yanyana) hataları engellemek için yükseltilmelidir.

### Not:

Ai tarafından önerilen değerler.

| Senaryo             | Önerilen Komut            | Koruma Seviyesi | Hata Toleransı              |
| ------------------- | ------------------------- | --------------- | --------------------------- |
| Standart Arşiv      | -l 12 -b 1M -p 16 -i 8    | Düşük           | Küçük bit hataları          |
| Güvenli Depolama    | -l 15 -b 4M -p 32 -i 32   | Orta            | Disk bozulmaları (Bit-rot)  |
| Derin Uzay (Zırhlı) | -l 19 -b 1M -p 64 -i 64   | Yüksek          | Radyasyon / SEU hataları    |
| Voyager (Ekstrem)   | -l 19 -b 1M -p 128 -i 128 | Maksimum        | Fiziksel medya parçalanması |
