# improved-octo-chainsaw
### öncelikle ayrı excel dosyalarında gönderilen verilerden ortak olanları belirleme ve eleme yapılır.
## olmayan paketler install packages("...") yöntemi ile kurulur ve library() yolu ile gerekşi paketler çalıştırılır.
install.packages("writexl")
library(readxl)
library(dplyr)
library(writexl)

## 2022 ve 2023'te ödeme alanlardan ortak olanları bularak yeni bir excel dosyasına dönüştürme işlemi.
df1 <- read_excel("..//Desktop/veriler/Yeni klasör/2022ödeme yapılan.xlsx")
df2 <- read_excel("..//Desktop/veriler/Yeni klasör/2023ödeme yapılan.xlsx")

ödeme_yapılan_ortak <- inner_join(df1, df2, by = "Tesis Adı")

write_xlsx(ödeme_yapılan_ortak, "..//Desktop/veriler/ödeme_yapılan_ortak.xlsx")

print



## 2022 ve 2023 yılında ödeme almayanlardan ortak olanları bularak yeni bir excel dosyasına dönüştürme işlemi.
df3 <- read_excel("..//Desktop/veriler/Yeni klasör/2022ödeme yapılmayan.xlsx")
df4 <- read_excel("..//Desktop/veriler/Yeni klasör/2023ödeme yapılmayan.xlsx")

ödeme_yapılmayan_ortak <- inner_join(df3, df4, by = "Tesis Adı")
write_xlsx(ödeme_yapılmayan_ortak, "..//Desktop/veriler/ödeme_yapılmayan_ortak.xlsx")



## 2022 yılında ödeme almayanlardan 2023 yılında ödeme alan var mı kontrolü, olanlar var ise yeni bir excel oluşturma ortak verilerden.
df5 <- read_excel("..//Desktop/veriler/Yeni klasör/2022ödeme yapılmayan.xlsx")
df6 <- read_excel("..//Desktop/veriler/Yeni klasör/2023ödeme yapılan.xlsx")

odemeyok2023var <- inner_join(df5, df6, by = "Tesis Adı")
write_xlsx(odemeyok2023var, "..//Desktop/veriler/odemeyok2023var.xlsx")

### hem 2022 yılında hem de 2023 yılında ödeme almayanlar ile 2022 yılında ödeme almayıp 2023 yılında alanlar bir excel sayfasında birleştirilir. oluşturulan excel'deki verilere yardımdan faydalanma durumunu gösteren bir sütun eklenmelidir. excel dosyasının adı eşleştirme konmuştur.
## yeni bir r script sayfası açılır ve kütüphaneden çalıştırılması gereken paketler çalıştırılır. Yoksa install.packages("..") yoluyla indirilir.

library(tidyverse)
library(ggdag)
library(dagitty)
library(broom)
library(MatchIt)
library(modelsummary)
library(dplyr)
library(readxl)
set.seed(1234)

## yukarıdaki işlemler ile hazırlanmış olan excel dosyası R'a tanıtılır
df6 <- read_excel("..//veriler/eşleştirme.xlsx")

## eşleştirme yapılır. 
eşleşmiş <- matchit(Yardım ~ City + AA + Çalışmasüresi + kapasitegün,
                    data = df6,
                    method = "nearest",
                    distance = "mahalanobis",
                    replace = TRUE)
summary(eşleşmiş)


eşleşmiş$match.matrix

matched_data <- match.data(eşleşmiş)

## eşleştirme sonucunda kontrol amaçlı eşleştirlienleri gösteren bir excel bilgisaya indirilebilir. 
library(writexl)
write_xlsx(matched_data, "matched_data.xlsx")

