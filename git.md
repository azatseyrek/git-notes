# Git Notları

## Terimler:

- **Staging Area (Hazır Alan):** `git add` komutuyla eklediğimiz alana "staging area" denir.

- **Working Directory (Çalışma Dizini):** Yazdığınız kodun henüz `git add .` yapılmamış hali, yani yazdığınız ancak henüz staging area'ya eklenmemiş olan kodun durumunu ifade eder.

## Git Reset & Git Revert

1) **Reset & Revert Arasındaki Fark:**
   - **Reset:** Commitleri silerek geri alınan duruma döner.
   - **Revert:** Commitleri silmez, tersine bir commit oluşturarak geri alınan değişiklikleri saklar.

2) **Reset --Hard Nedir?**
   - Diyelim ki bir değişiklik yaptık, birkaç satır kod ekledik ve bunları `git add` ve `git commit` ile kaydettik. Bu durumda commitlerimiz arasında yaptığımız son değişiklik görünüyor.
   - İstenilen durum: Yaptığımız değişikliği geri almak ve commitleri de history'den kaldırmak istiyoruz. İşte bu durumda `git reset` komutunu kullanırız.
     - `git reset --hard`: Bu seçenekte, history'den commit silinir ve aynı zamanda yapılan değişiklikler working directory'den de kaldırılır.
     - `git reset`: Sadece `git reset` komutu kullanılırsa, history'den commit silinir, ancak yapılan değişiklikler working directory'de görünür.
     - `git reset --soft`: Yapılan değişiklik history'den gitmez, ancak working directory ve staged area'da kalır.

### Özet:
Diyelim ki bir değişiklik yaptık ve bunu `git add` ile staged area'ya ekledik, sonra commitledik. Artık git history'mizde görünüyor. Sonra dedik ki bunu geri alalım. Bu durumda 4 senaryo var:
1. Revert kullanmak: Tüm değişiklikleri geri alır, ancak history'de kalır ve istenildiğinde geri alınabilir.
2. Reset kullanmak: Belirtilen senaryoya göre farklı reset seçenekleri kullanılır:
   - `git reset`: Yapılan değişiklikler working directory'de kalır, ancak staged area'dan silinir.
   - `git reset --soft`: Yapılan değişiklikler history'den silinmez, ancak working directory ve staged area'da kalır.
   - `git reset --hard`: History'den, staged area'dan ve working directory'den tamamen silinir.

## Merge Çeşitleri
- **Fast-forward ve Non-Fast Forward:** Merge işlemi, genellikle fast-forward merge olarak yapılır. Bu durumda, bir master branch'imiz olduğunu varsayalım. Bir dev branch'ı açtık ve dev'de değişiklikler yaptık, commitledik. Daha sonra master'a gittiğimizde `git merge dev` dediğimizde fast-forward şeklinde merge işlemi yapılır. Buradaki asıl mesele, merge işleminden sonra dev branch'ının commitleri de master'a eklenir. Bu durumu istemiyorsak `git merge --squash dev` veya non-fast-forward merge (recursive merge) kullanabiliriz.
  - **`git merge --squash dev` Kullanımı:** Bu şekilde merge yaparsak en son bir commit daha atmamız gerekir. Bu genellikle dev'in master'a merged to master gibi bir durumunda kullanılır. Yani squash ile merge işlemi yaparsak, dev'deki commitler yerine bizim en son atacağımız commitler görünecektir.
  - **`git merge --no-ff dev` Kullanımı:** Burada dev'deki commitler de gelir, ancak sadece `git merge dev` dediğimiz gibi değil, merge edildiğine dair bir commit message da eklenir.
