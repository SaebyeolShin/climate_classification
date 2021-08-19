# [DACON] 자연어 기반 기후기술분류 AI 경진대회 4등 코드 

대회 설명: 국가 연구개발과제를 '기후기술분류체계'에 맞추어 라벨링하는 알고리즘 개발

참고자료:
[기후기술 분류체계](https://www.ctis.re.kr/ko/techClass/classification.do?key=1141) ,
[2019년 기후기술 국가연구개발사업 조사분석 보고서](https://www.gtck.re.kr/gtck/gtcPublication.do?mode=view&articleNo=1844&article.offset=30&articleLimit=10)


주최 : 녹색기술센터(GTC)

주관 : DACON [대회 링크](https://dacon.io/competitions/official/235744/overview/description)

## Path
```
├── data_j
│   ├── original (download dataset in dacon site)
│   │   ├── train.csv
│   │   └── test.csv
│   │
│   └── category_add
│       └── custom_labels_mapping.csv
│
├── utils_j.py
│
├── preprocess_j.py (tokenizer)
│
├── dataset_j.py (model + middle category model)
├── model_j.py (bert model & custom XLMRoberta)
│
├── train_j.py (train)
└── inference_j.py (inference)

====================================================

├── data_s (download dataset in dacon site)
│   ├── train.csv
│   └── test.csv
│ 
├── preprocessing_s.py (tokenizer)
│
├── model_s.py (model)
│
├── train_s.py (train)
└── inference_s.py (inference)

====================================================

├── sample_submission.csv
├── inference.py (ensemble j+s)
└── 1_2_6_10.csv (submission csv)

```
## How to Use

NewStar 팀은 두명이 서로 다른 모델을 사용하였기 때문에 코드의 파이프라인은 크게 2가지로 나누었습니다.  

1. train_j.py -> inference_j.py
2. train_s.py -> inference_s.py

두 개의 파이프라인이 끝나면 inference.py를 실행하여 최종 csv파일이 생성됩니다.

위의 과정을 모두 포함한 스크립트 파일을 실행하면 됩니다.

```bash
bash run.sh
```

파일 용량이 큰 관계로 코드와 중분류 카테고리 추가를 위한 자료만 첨부합니다. 

다음 [링크](https://drive.google.com/drive/folders/1DXJkhQr3Eybut7XrBgjBt-GM3apNoWHM)에서 train.csv, test.csv를 data_s와 data_j/original/에 넣으면 됩니다.

============================================================================

Since the team NewStar used two different models, the code pipeline was divided into two major parts.

1. train_j.py -> inference_j.py
2. train_s.py -> inference_s.py

At the end of both pipelines, the final csv file is generated by running inference.py.

All you have to do is run the script file that includes all of the above steps.

```bash
bash run.sh
```
Due to the large file size, only the code and material for middle-category are attached.
Download train.csv and test.csv from [link](https://drive.google.com/drive/folders/1DXJkhQr3Eybut7XrBgjBt-GM3apNoWHM) and then put them in data_s and data_j/original/.


## System Requirements
- python=3.8  
- tokenizers=0.10.3     
- torch=1.9.0  
- transformers=4.8.1  
- pandas=1.2.5  

## Description

```
- klue/roberta-large
  ├── tokenizer = AutoTokenizer.from_pretrained('klue/roberta-large')
  └── model = XLMRobertaForSequenceClassification.from_pretrained('klue/roberta-large', num_labels=46)
```
```
- bert-base-multilingual-cased
  ├── tokenizer = BertTokenizer.from_pretrained('bert-base-multilingual-cased')
  └── model = BertForMultiLabelSequenceClassification.from_pretrained('bert-base-multilingual-cased', num_labels=46, return_dict=False)
```
```
- klue/roberta-base (with middle category targets)
  ├── tokenizer = AutoTokenizer.from_pretrained('klue/roberta-large')
  └── model = custom_XLMRoberta('klue/roberta-base', n_classes=46, n_middle_classes=15)
```
```
- kykim/funnel-kor-base
  ├── tokenizer = ElectraTokenizer.from_pretrained('kykim/funnel-kor-base')
  └── model = FunnelForSequenceClassification.from_pretrained('kykim/funnel-kor-base', num_labels=46)
```

