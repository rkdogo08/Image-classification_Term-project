1. Q : 랜덤 포레스트를 학습하는데 SVM에 비해 어느 정도 시간이 짧게 걸렸는지 궁금합니다.

   A : Level0에서 학습을 위해 SVM GPU를 이용하였는데 이 방법이 비정상적으로 오래 걸렸던 것으로 linesrSVM을 이용하게 되면 굉장히 빠른 속도로 결과를 보여줍니다. SVM에는 튜닝해야 할 파라미터가 있으므로 그리드 서치를 이용하게 되면 C에대한 파라미터, gamma 파라미터 각각 2개만 넣어주어도 아래와 같이 랜덤 포레스트보다 많은시간이 걸립니다. 
   
   | Level | codebook size | image size | step size  | model | Etc | time(s) |score | 
   |:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
   | 0 | 400 |  256x256 | 8 | Random Forest | n_estimators=1000 | 46.8 |0.39361 | 
   | 0 | 400 |  256x256 | 8 | SVM | linear | 0.40602 | 9.0 |
   | 0 | 400 |  256x256 | 8 | SVM GridSearch| rbf | 128.2 |0.38120 |
   | 0 | 400 |  256x256 | 8 | SVM_GPU | linear |  333.5 |-- |
   
  
   
2. Q : 코드북 사이즈를 1200까지 하셨는데 코드북 사이즈를 올리면서 실행 시간이 오래걸리지는 않았는지 궁금합니다.

   A : Level2, 코드북 사이즈가 1200 일때, 데이터 로드 이후부터 학습 이후까지의 시간을 측정하였더니 1639.814초(약 27분)가 걸렸습니다.
   같은 방식으로 코드북 사이즈가 400일때는 759.408초(약 13분)가 걸렸습니다.
   
3. Q : 차원축소는 어느 부분에 사용하고 어떤 함수를 이용하였는지 궁금합니다.

   A :  PCA는 SIFT를 통해 뽑은 피쳐에 이용하였으며 PCA함수는 아래와 같습니다.
   ![image](https://user-images.githubusercontent.com/46476876/70971024-52616100-20e3-11ea-97e4-9d3a0bb357f3.png)
   ```python
   from sklearn.decomposition import PCA
   def PCA_(train_des,test_des,n):
       train_des=np.array(train_des).reshape(-1,128)
       test_des=(np.array(test_des)).reshape(-1,128)

       pca=PCA(64)
       train_des=pca.fit_transform(train_des)
       test_des=pca.transform(test_des)

       train_des=train_des.reshape(3030,1024,-1)
       test_des=test_des.reshape(1692,1024,-1)

       return train_des,test_des
   ```

