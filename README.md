# Top1–Top2 Olasılık Farkı ile Cevap Doğruluğu Tahmini

**Hesaplamalı Anlambilim Dersi – Konu 4 Final Projesi**  
Betül Arslan

## Proje Özeti

Bu proje, Cosmos-T1 (`ytu-ce-cosmos/Turkish-Gemma-9b-T1`) modelinin Türkçe matematik sorularına verdiği cevapların doğruluğunu, modelin üretim sürecindeki **top1–top2 token olasılık farklarından** çıkarılan istatistiksel özellikler kullanarak tahmin etmeyi amaçlamaktadır.

## Hipotez

Doğru cevap üreten model, doğru yolda ilerlerken genel olarak daha **emin** (yüksek top1–top2 fark) tokenlar üretir; yanlış cevaplarda bu güven daha düşük ya da daha değişken olur.

## Yöntem

1. `gsm8k_tr` veri kümesinden 1200 soru seçildi
2. Cosmos-T1 ile `output_scores=True` kullanılarak her token adımında top1–top2 farkı hesaplandı
3. Her cevap için **18 istatistiksel özellik** çıkarıldı
4. Dengeli veri kümesi oluşturuldu (640 eğitim / 100 test)
5. Logistic Regression, Random Forest ve Gradient Boosting modelleri karşılaştırıldı

## Sonuçlar

| Model | Acc | F1 | AUC |
|---|---|---|---|
| Logistic Regression | 0.670 | 0.7027 | 0.756 |
| **Random Forest** | **0.740** | **0.7679** | **0.8032** |
| Gradient Boosting | 0.730 | 0.7523 | 0.792 |

En önemli özellik: `mean_first10` (ilk 10 tokenin ortalama güveni)

## Dosyalar

| Dosya | Açıklama |
|---|---|
| `top1_top2_project.ipynb` | Tüm kod: veri üretimi, özellik çıkarma, ML modelleri |
| `rapor_ieee.tex` | IEEE formatında LaTeX raporu |
| `sunum.pptx` | Sunum (10 slayt) |
| `figures/` | Grafik görseller (confusion matrix, ROC, feature importance, dağılım) |

## Kurulum

```bash
pip install transformers torch datasets scikit-learn pandas numpy matplotlib seaborn
```

Model: `ytu-ce-cosmos/Turkish-Gemma-9b-T1` (Gemma-2 9B, ~18.5 GB VRAM gerektirir)  
Donanım: Google Colab L4 GPU önerilir

## Veri Kümesi

- **Kaynak:** `ytu-ce-cosmos/gsm8k_tr` (8768 temiz Türkçe matematik sorusu)
- **Üretim:** 1200 soru, MAX_NEW_TOKENS=128, batch_size=8
- **Doğruluk:** %30.8 (thinking modeli kısıtından kaynaklanan teknik sınırlılık)
