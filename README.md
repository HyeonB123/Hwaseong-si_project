# 화성시 고령화 문제와 노인 복지시설 최적 입지 분석
배경 1) 화성시의 인구 급증으로 인한 특례시 지정 -> 고령 인구 유입으로 인한 고령화 사회 우려
    2) 화성시의 고령 인구 비율은 꾸준히 증가하고 있으며, 2023년에는 10% 돌파
    3) 다른 특례시에 비해 노인 복지시설이 적은 편에 속함

목적: 화성시 내 고령화 문제를 대비하여 노인 복지시설 설치를 위한 최적의 입지 분석

기간: 2024.10 ~ 2024.12 (약 2개월)
인원: 2명
역할: 데이터 수집 및 전처리, Tableau를 통한 분석 결과 시각화, Clustering 기법 사용
습득 스킬: Tableau를 활용한 시각화 능력, Excel과 Python을 활용한 데이터 전처리 및 Clutering 기법 활용법 습득

<img width="411" height="175" alt="Image" src="https://github.com/user-attachments/assets/b6bb55cf-196f-4b91-a92a-0aaf98d05534" />


## 사용 데이터
- 총 12개의 데이터 사용
- 사용된 데이터는 행정구역(읍/면/동)별 5세 단위 주민등록인구를 제외하고, 모두 노인복지시설 인근에 위치할 경우 노인들에게 도움이 될 만한 요소들을 기준으로 선정
- 각 데이터가 읍/면/동 별로 얼마나 분포되어 있는지 수치화하여 전처리 진행
- 1차 Clutering을 위해 고령화 지수라는 파생변수 생성

<img width="491" height="199" alt="Image" src="https://github.com/user-attachments/assets/31f49a65-7947-4f7b-9a8a-1d6f21172c93" />


## 1차 클러스터링
- 고령화 지수와 65세 이상 인구수 두 개의 변수만을 고려하여 K-medoids Clustering 진행
- Cluster1에 포함된 지역은 고령화지수가 상대적으로 낮지만 Cluster0 지역보다 65세 이상 인구수가 포함된 지역이 많기 때문에, Cluster1이 포함된 지역 18개를 선정

<img width="320" height="169" alt="Image" src="https://github.com/user-attachments/assets/1f4a6071-986f-43a8-9bcf-ec1ec6dc4293" />


## 다중 공선성 확인
- 1차 클러스터링 이후, 선정된 지역들을 대상으로 11개의 변수를 모두 활용할 예정
- 다중공선성 확인을 통해 분석의 정확성과 신뢰성을 높임
- VIF가 10을 초과한 변수들 중 CCTV와 의료기관은 노인 복지시설 입지 분석에 주요한 변수이므로 제거가 아닌 PCA 분석을 통해 변수를 축소하는 방법을 채택

<img width="117" height="124" alt="Image" src="https://github.com/user-attachments/assets/4223a698-cd90-4a33-8093-8743a0d218f5" />


## 주성분 분석
- Scree Plot과 누적 설명된 분산 비율을 확인하여 n_components = 2로 설정
- Scree Plot을 확인해보았을때 완만해지는 구간은 n=3일 때지만, 누적 설명된 분산 비율이 0.76으로 충분한 수치이며, 차원이 적을수록 해석과 시각화가 쉬워지고, 노이즈를 줄일 수 있기 때문에 n_components=2일 때로 설정
- PC1와 PC2 두 개의 주요 주성분을 생성
- PC1은 주로 안전성 및 생활 편의성을 나타냄. 반면, PC2는 주로 노인 복지와 사회적 지원을 나타냄
- 두 개의 주성분 PC1, PC2로 전체 데이터의 약 77%를 설명 가능. 주요 정보를 잃지 않으면서 차원을 줄이려면 설명된 분산의 수치가 70%이상이면 괜찮다고 판단

<img width="350" height="113" alt="Image" src="https://github.com/user-attachments/assets/b3a4988a-c491-4936-9014-cd1a49bd52ce" />


## WSM(Weight Sum Method)
- 고령화지수, 의료기관, 버스정류장, 공원 수, 의료기관 수 총 5개의 변수를 활용하여 WSM 수행
- 최종적으로 향남읍, 남양읍, 봉담읍, 동탄7동, 진안동이 최적 지역으로 선정

<img width="358" height="230" alt="Image" src="https://github.com/user-attachments/assets/dd7ad81a-545f-438c-8a7c-3b36b1a9fa5b" />


## 2차 클러스터링
- K-means, Hierachical Clustering, K-medoids 기법을 비교 분석하여 최종적으로 K-medoids Clustering을 채택
- 실루엣점수 계산 결과 k=2에서 가장 높은 점수를 보여 K의 값을 2로 결정
- K-medoids의 결과로 11개의 지역 선정

<img width="78" height="99" alt="Image" src="https://github.com/user-attachments/assets/12ef5241-9548-499d-a5ad-357d507e2241" />


## 최종 선정 지역
- 기존 노인 복지시설 수를 고려하여 과잉 지역 제외 -> 최종적으로 진안동과 동탄7동 선정
- 최종적으로 구글 위성지도를 살펴보며 주변 인프라나 교통 접근성 및 편의시설을 확인하여 최종적인 입지 선정

<img width="488" height="134" alt="Image" src="https://github.com/user-attachments/assets/fbcd52e2-aeb5-4aa9-ac38-e5763fa8f227" />

## 한계 및 추가적인 연구방안
- 실제 노인복지시설 입지 선정에는 노인 인구, 고령화지수, 교통 접근성, 의료기관 등 다양한 변수를 고려했으나, 추가적으로 필요한 데이터가 많다고 생각되었지만 생각했던 데이터들이 화성시에서 제공하지 않는 경우가 많았기 때문에 추가적인 변수를 고려하기 힘들었음
- 노인 및 가족 대상 설문조사, 인터뷰 등 정성적 데이터 수집을 통해 실제 욕구와 생활패턴을 반영한 분석이 추가적으로 필요해 보임
- 분석 결과를 실제 행정업무, 예산, 인력 등 정책 실행 가능성과 연계해 검증해볼 필요가 있음
- 불필요하게 복잡한 변수나 분석 단계를 줄이고, 실제 의사결정에 직관적으로 활용할 수 있는 모델로 개선하는 것이 필요해 보임. 또한 GIS(지리정보시스템) 등 시각화 도구 활용을 확대하는 것이 필요해 보임
