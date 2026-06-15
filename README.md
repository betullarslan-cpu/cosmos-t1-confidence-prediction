# Top1-Top2 Olasılık Farkı ile Cevap Doğruluğu Tahmini

Hesaplamalı Anlambilim Dersi Final Projesi

## Ne yaptım?

Cosmos-T1 modelini gsm8k_tr veri kümesindeki sorulara çalıştırdım. Modelin her token üretirken en yüksek iki olasılık arasındaki farkı kaydettim (top1-top2 diff). Bu fark dizisinden 18 tane istatistiksel özellik çıkardım ve bunları kullanarak modelin cevabının doğru mu yanlış mı olduğunu tahmin etmeye çalıştım.

## Nasıl çalışır?

Model bir token üretirken aslında tüm kelimeler için olasılık hesaplıyor. En yüksek iki olasılık arasındaki fark modelin ne kadar emin olduğunu gösteriyor. Doğru cevaplarda bu güven daha yüksek olmalı diye düşündüm — ve bir ölçüde öyle çıktı.

## Sonuçlar

1200 soru ürettim, 370 tanesi doğruydu (%30.8). Random Forest en iyi sonucu verdi:

| Model | Accuracy | F1 | AUC |
|---|---|---|---|
| Logistic Regression | 0.67 | 0.70 | 0.76 |
| **Random Forest** | **0.74** | **0.77** | **0.80** |
| Gradient Boosting | 0.73 | 0.75 | 0.79 |

En belirleyici özellik `mean_first10` çıktı — ilk 10 tokenin ortalama güveni.

## Dosyalar

- `top1_top2_project.ipynb` — kodun tamamı
- `rapor_ieee.tex` — IEEE formatında rapor
- `sunum.pptx` — sunum slaytları
- `figures/` — grafikler

## Çalıştırmak için

Google Colab'da L4 GPU gerekiyor (model ~18.5 GB VRAM kullanıyor).

```bash
pip install transformers torch datasets scikit-learn pandas numpy matplotlib seaborn
```
