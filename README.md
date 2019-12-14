# Image-classification_Term-project

- 데이터 로드
```python
df_data=pd.read_csv('/content/Label2Names.csv',header=None)
class_name=df_data[1].tolist()
train_des,label=load_train_data(256,8)
test_des,test_name=load_test_data(256,8)
```
데이터를 로드하고 클래스별로 라벨링한다.

- validation 셋 생성
```python
X1,X2,y1,y2=make_val(train_des,label,303)
```

- PCA
```python
train_des,test_des=PCA_(train_des,test_des,64)
des_vec=train_des.reshape(-1,64)
```
- 코드북 생성
```python
codebooksize=1200
seeding = kmc2.kmc2(des_vec, codebooksize) 
Kmeans = MiniBatchKMeans(codebooksize, init=seeding,init_size=codebooksize).fit(des_vec)
codebook = Kmeans.cluster_centers_
print("codebook : ",codebook.shape)
```
kmeans를 이용하여 코드북을 생성한다.


- SPM 
```python
h_list = histogram_spm(2, train_des,codebooksize)
test_h = histogram_spm(2, test_des,codebooksize)
```

- Histogram 그랴프
```python
histo_plt=h_list[0].reshape(21,codebooksize)
plt.bar(range(codebooksize),histo_plt[0,:] , color='g')

fig,ax =plt.subplots(2,2)
for i,axi in enumerate(ax.flat):
  axi.bar(range(codebooksize),histo_plt[i+1,:] , color='g')

fig,ax =plt.subplots(4,4)
for i,axi in enumerate(ax.flat):
  axi.bar(range(codebooksize),histo_plt[i+5,:] , color='g')
```

< Level0 >
![image](https://user-images.githubusercontent.com/46476876/70849964-93315e00-1ec8-11ea-84b1-67a3c7aa6df6.png)

 < Level1 > 
![image](https://user-images.githubusercontent.com/46476876/70849962-8a408c80-1ec8-11ea-9315-8ba9cafb6483.png)


   < Level2 >
![image](https://user-images.githubusercontent.com/46476876/70849950-7a28ad00-1ec8-11ea-849e-68bfec32efb4.png)



- 정규화
```python
scaler=StandardScaler()
h_list=scaler.fit_transform(h_list)
test_h=scaler.transform(test_h)
```
- 학습 모델(Histogram Intersection)
```python
results=make_histogramIntersection(h_list,test_h)
```

- csv 파일 생성
```python
make_result_csv(results,test_name)
```

# 성능
- 다양한 학습모델 비교

| Level | codebook size | image size | step size  | model | Etc | score |
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| 0 | 400 |  256x256 | 8 | Random Forest | n_estimators=1000 | 0.41193 |
| 0 | 400 |  256x256 | 8 | Decision Tree+ Bagging | n_estimators=1000 | 0.41193 |
| 0 | 400 |  256x256 | 8 | SVM | rbf, C=4, gamma=0.001 | 0.41193 |

- 이미지/스텝크기 비교

| Level | codebook size | image size | step size  | model | Etc | score |
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| 0 | 400 |  64x64 | 4 | SVM | linear,C=1 | 0.31028 |
| 0 | 400 |  128x128 | 4 | SVM | linear,C=1 | 0.35992 |
| 0 | 400 |  128x128 | 5 | SVM | linear,C=1 | 0.37352 |
| 0 | 400 |  256x256 | 8 | SVM | rbf, C=4, gamma=0.001 | 0.41193 |

- 차원축소

| Level | codebook size | image size | step size  | PCA | model | Etc | score |
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| 0 | 400 |  256x256 | 8 | X | SVM | rbf, C=4, gamma=0.001 | 0.41193 |
| 0 | 400 |  256x256 | 8 | O | PCA, SVM | rbf, C: 4, gamma: 0.001 | 0.43971 |
| 0 | 400 |  256x256 | 8 | LDA | SVM | RAM crash | -- |
| 2 | 600 |  256x256 | 8 | X | SVM | rbf, C: 4, gamma: 0.001 | 0.58806 |
| 2 | 600 |  256x256 | 8 | O | SVM | rbf, C: 4, gamma: 0.001 | 0.58806 |

- 정규화

| Level | codebook size | image size | step size  | sclae | model | Etc | score |
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| 0 | 400 |  256x256 | 8 | X | SVM | rbf, C=4, gamma=0.001 | 0.41193 |
| 0 | 400 |  256x256 | 8 | O | SVM | rbf, C=4, gamma=0.001 | 0.43026 |
| 0 | 400 |  256x256 | 8 | X | PCA, SVM | rbf, C: 4, gamma: 0.001 | 0.43971 |
| 0 | 400 |  256x256 | 8 | O | PCA, SVM | rbf, C: 4, gamma: 0.001 | [0.4444](https://github.com/rkdogo08/Image-classification_Term-project/blob/master/code/term_project_PCA_scale.ipynb) |
| 2 | 600 |  256x256 | 8 | X | SVM | Histogram Intersection | 0.59633 |
| 2 | 600 |  256x256 | 8 | O | SVM | Histogram Intersection | 0.60342 |
| 2 | 1200 |  256x256 | 8 | X | SVM | Histogram Intersection | 0.60106 |
| 2 | 1200 |  256x256 | 8 | O | SVM | Histogram Intersection | 0.60874 |



- SPM

| Level | codebook size | image size | step size  | model | Etc | score |
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| 0 | 400 |  256x256 | 8 | SVM | rbf, C=4, gamma=0.001, scaler | [0.43026](https://github.com/rkdogo08/Image-classification_Term-project/blob/master/code/term_project_level0.ipynb) (베이스코드) |
| 1 | 400 |  256x256 | 8 | SVM | Histogram Intersection | 0.54078 |
| 2 | 200 |  256x256 | 8 | SVM | Histogram Intersection| 0.56264 |
| 2 | 400 |  256x256 | 8 | SVM | Histogram Intersection| 0.58747 |
| 2 | 600 |  256x256 | 8 | SVM | Histogram Intersection| 0.58806 |
| 2 | 1200 |  256x256 | 8 | SVM | Histogram Intersection | [0.60874](https://github.com/rkdogo08/Image-classification_Term-project/blob/master/code/term_project_level2_1200.ipynb) (코드) |



- SPM 가중치 변화

 | level0 |  level1 | level2 | score |
|:--:|:--:|:--:|:--:|
 | 0.25 |  0.25| 0.5 | 0.58806 |
 | 0.25 |  0.5| 1 | 0.59633 |
