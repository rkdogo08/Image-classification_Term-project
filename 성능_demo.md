
| Level | codebook size | image size | step size  | model | Etc | score |
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| 0 | 400 |  256x256 | 8 | SVM | rbf, C=4, gamma=0.001 | 0.41193 |
| 0 | 400 |  256x256 | 8 | Random Forest | n_estimators=1000 | 0.41193 |
| 0 | 400 |  256x256 | 8 | Decision Tree+ Bagging | n_estimators=1000 | 0.41193 |
| 0 | 400 |  256x256 | 8 | PCA, SVM | rbf, C: 4, gamma: 0.001 | [0.43971]() |
| 0 | 400 |  256x256 | 8 | LDA, SVM | RAM crash | -- |
| 1 | 400 |  256x256 | 8 | SVM | C=1| 0.54078 |
| 2 | 200 |  256x256 | 8 | SVM | C=1| 0.56264 |
| 2 | 400 |  256x256 | 8 | SVM | C=1| 0.58747 |
| 2 | 600 |  256x256 | 8 | SVM | C=1 | [0.58806]() |

| 0 | 400 |  256x256 | 8 | SVM | rbf, C=4, gamma=0.001 | 0.43026 |
| 0 | 400 |  256x256 | 8 | SVM | kernel='precomputed' | 0.44562 |
