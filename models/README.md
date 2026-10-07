# Eğitilmiş Model Dosyası

Bu klasörde, İHA görevlerinin enerji ihtiyacını tahmin etmek amacıyla geliştirilen optimize edilmiş gradyan artırma regresyon modeli bulunmaktadır.

## Model Dosyası

- `optimize_gradyan_artirma_modeli.joblib`

Model, sentetik görev verileri kullanılarak eğitilmiştir. Beş katlı çapraz doğrulama ve rastgele hiperparametre araması uygulanmıştır.

Bağımsız test kümesindeki model sonuçları:

- MAE: 8,9065 Wh
- RMSE: 12,3851 Wh
- R²: 0,9758

Model bir kavram kanıtlama çalışmasıdır. Gerçek sistemlerde kullanılmadan önce gerçek cihaz verileriyle yeniden eğitilmesi ve doğrulanması gerekmektedir.
