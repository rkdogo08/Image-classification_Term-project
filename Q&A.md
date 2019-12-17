1. Q : 랜덤 포레스트를 학습하는데 SVM에 비해 어느 정도 시간이 짧게 걸렸는지 궁금합니다.

   A : 아래와 같이 DT+Baggin이 SVM보다 르며 Gridsearch를 이용하면 학습시간이 더 오래걸리게 됩니다.
   
   추가적으로 DT+Baggin와 SVM의 성능을 다시 측정하였습니다.
2. Q : 코드북 사이즈를 1200까지 하셨는데 코드북 사이즈를 올리면서 실행 시간이 오래걸리지는 않았는지 궁금합니다.

   A : 
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

