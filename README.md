# Image-classification_Term-project





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

| Level | codebook size | image size | step size  | model | Etc | score |
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| 0 | 400 |  256x256 | 8 | SVM | rbf, C=4, gamma=0.001 | 0.41193 |
| 0 | 400 |  256x256 | 8 | PCA, SVM | rbf, C: 4, gamma: 0.001 | 0.43971 |
| 0 | 400 |  256x256 | 8 | LDA, SVM | RAM crash | -- |
| 2 | 600 |  256x256 | 8 | SVM | C=1 | 0.58806 |
| 2 | 600 |  256x256 | 8 | PCA, SVM | C=1 | 0.58806 |

- 정규화

| Level | codebook size | image size | step size  | model | Etc | score |
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| 0 | 400 |  256x256 | 8 | SVM | rbf, C=4, gamma=0.001 | 0.41193 |
| 0 | 400 |  256x256 | 8 | SVM | rbf, C=4, gamma=0.001, scaler | [0.43026](https://github.com/rkdogo08/Image-classification_Term-project/blob/master/code/term_project_level0.ipynb) |
| 0 | 400 |  256x256 | 8 | PCA, SVM | rbf, C: 4, gamma: 0.001 | 0.43971 |
| 0 | 400 |  256x256 | 8 | PCA, SVM | rbf, C: 4, gamma: 0.001, scaler | [0.4444](https://github.com/rkdogo08/Image-classification_Term-project/blob/master/code/term_project_PCA_scale.ipynb) |
| 2 | 600 |  256x256 | 8 | SVM | C=1 | 0.59633 |
| 2 | 600 |  256x256 | 8 | SVM | C=1, scale | 0.60874 |



- SPM

| Level | codebook size | image size | step size  | model | Etc | score |
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| 1 | 400 |  256x256 | 8 | SVM | C=1| 0.54078 |
| 2 | 200 |  256x256 | 8 | SVM | C=1| 0.56264 |
| 2 | 400 |  256x256 | 8 | SVM | C=1| 0.58747 |
| 2 | 600 |  256x256 | 8 | PCA, SVM | C=1 | 0.58806 |
| 2 | 600 |  256x256 | 8 | SVM | C=1 | [0.58806]() |

-가중치 변화

 | level0 |  level1 | level2 | score |
|:--:|:--:|:--:|:--:|
 | 0.25 |  0.25| 0.5 | 0.58806 |
 | 0.25 |  0.5| 1 | 0.59633 |
