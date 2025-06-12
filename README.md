# AdaSampah - ML Model for Clean/Dirty Classification
Sebuah proyek pembelajaran mesin untuk mengklasifikasikan sampah ke dalam 2 kategori yang berbeda menggunakan Convolutional Neural Networks (CNN) dengan pembelajaran transfer (transfer learning). 

---
## Overview
Proyek ini mengimplementasikan sistem gambar dengan menggunakan teknik deep learning. Model ini dapat mengklasifikasikan gambar tempat ke dalam 2 kategori yang berbeda, yaitu bersih (0) atau kotor (1). Model akan mencoba mengklasifikasikan apakah sebuah gambar tersebut tempat yang bersih atau kotor.

---
## Dataset

Proyek ini menggunakan [dataset](https://www.kaggle.com/datasets/ibnurajuhumam/bersih-kotor-dataset) dari Kaggle dengan 2 kelas, yang berisi lebih dari 5400 gambar yang terdistribusi sebagai berikut:

1. clean: 2700
2. dirty: 2700

### **Contoh Gambar untuk Masing-Masing Kelas**
1. Dirty Data

   ![dirty-data](https://github.com/user-attachments/assets/fc55eb51-4a2f-4825-9965-0f4696a827c5)

3. Clean Data

   ![clean-data](https://github.com/user-attachments/assets/e546af13-dfd4-4a0a-9c66-376d368fee62)

---
## Data Distribution
Dataset dalam proyek ini dipecah dengan komposisi 80 (training), 10 (validation), 10 (testing)

| Set   | Label | Jumlah |
|-------|-------|--------|
| Test  | Clean | 259    |
| Test  | Dirty | 281    |
| Train | Clean | 2177   |
| Train | Dirty | 2143   |
| Val   | Clean | 264    |
| Val   | Dirty | 276    |

---

## Model Architecture and Training

Model ini menggunakan **Transfer Learning** dengan EfficientNetB0 sebagai base model:

### Model Configuration
- **Base Model**:
  - EfficientNetB0 pretrained on ImageNet
  - Input shape: (224, 224, 3)
  - `include_top=False` (tanpa fully connected layer bawaan)
  - All layers frozen (`trainable=False`)
- **Input Shape**: 224×224×3 RGB images
- **Additional Layers**:
  - GlobalAveragePooling2D
  - Dense layer (128 units) with ReLU activation
  - BatchNormalization layer
  - Dropout layer (0.3)
  - Output layer (1 unit) with Sigmoid activation (for binary classification)

### Training Parameters for Actual Training
- **Optimizer**: Adam (learning rate: 0.0005)
- **Loss Function**: Binary Crossentropy.
- **Epochs**: 30
- **Callbacks**: 
   - earlystop (EarlyStopping): Memantau kehilangan validasi dan menghentikan pelatihan lebih awal (pencegahan overfitting)
   - checkpoint (ModelCheckpoint): callback Keras yang digunakan untuk menyimpan model saat pelatihan, biasanya saat validasi mencapai performa terbaik.
   - accuracy_threshold (LambdaCallback): callback yang digunakan untuk menghentikan pelatihan secara otomatis jika akurasi dan val_accuracy sudah lebih besar dari 95%.

### Model Performance for Actual Training
- **Initial Training Accuracy**: 0.8270 (Epoch 1)
- **Final Training Accuracy**: 0.9789
- **Validation Accuracy**: 0.9537~

---
## Model Architecture and Fine-Tuning 

Model ini menggunakan **Transfer Learning** dengan EfficientNetB0 sebagai base model:

### Model Configuration
- **Base Model**:
  - EfficientNetB0 pretrained on ImageNet
  - Input shape: (224, 224, 3)
  - Layer ke-0 hingga ke-99 dibekukan (`trainable=False`) -> Hanya fine-tune dari layer ke-100 ke atas 
- **Input Shape**: 224×224×3 RGB images

### Training Parameters for Fine-Tuning
- **Optimizer**: Adam (learning rate: 0.00001)  
- **Loss Function**: Binary Crossentropy  
- **Epochs**: 10  
- **Callbacks**:  
   - earlystop (EarlyStopping): Memantau kehilangan validasi dan menghentikan pelatihan lebih awal untuk mencegah overfitting  
   - accuracy_threshold (LambdaCallback): Callback yang menghentikan pelatihan secara otomatis jika akurasi dan val_accuracy sudah lebih besar dari 95%  

### Model Performance for Fine-Tuning
- **Initial Training Accuracy**: 0.8382 (Epoch 1)  
- **Final Training Accuracy**: 0.9474
- **Final Validation Accuracy**: 0.9370  
- **Testing Accuracy**: 0.9574 
---




