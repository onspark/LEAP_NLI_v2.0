# LEAP NLI v2.0 dataset

GitHub repository for LEAP NLI v2.0, a dataset for Korean Legal Inference

## Dataset

### Format

The dataset files are all in JSON format. See the sample below:

```{json}
    {
        "guid": "c0073_h0422_mtlN_no001",
        "sentence1": "이 사건 범행에 사용된 공업용 커터칼은 [...] 피해자들에 대한 진단서 및 의무기록을 통하여 확인되는 상처의 정도도 결코 가볍지 않다.",
        "sentence2": "피해자가 1명 이상이다.",
        "gold_label": "entailment"
    }
```

The `guid` consists of a context identifier('c0073'), hypothesis identifier('h0422'), augmentation method('mtlN') and the creation number ('no001'). The `sentence1` field represents the context, while `sentence2` represents the either human-generated or augmented hypothesis. There are **four** augmentation methods used in this dataset: 

* mtlN: No machine translation, i.e. human-generated
* mtlG: Augmented using Google Translate
* mtlK: Augmented using Kakao i
* mtlP: Augmented using Naver Papago
  
For further details regarding the augmentation used in this dataset, refer to [** Augmentation Methods **](#augmentation-methods)

### Details

| dataset/mtl | train | dev  | test |
| :---------- | :---- | :--- | :--- |
| mtlN        | 773   | 70   | 60   |
| mtlG        | 4099  | 350  | 367  |
| mtlK        | 3743  | 300  | 311  |
| mtlP        | 2960  | 280  | 262  |
| Total       | 11575 | 1000 | 1000 |

* **Total: 13,575**

All 1,000 samples in the `test` dataset have been human-verified.

### Labels

* Entailment: 4,525
* Contradiction: 4,525
* Neutral: 4,525

Note that this is a machine-augmented dataset purposefully balanced. 

## Augmentation Methods

This dataset underwent augmentation from the human-generated 1.3k hypotheses using a combination of EDA(Easy Data Augmentation) and RTT(Round-trip-translation) methods. We used [KoEDA](https://github.com/toriving/KoEDA) and [KorEDA](https://github.com/catSirup/KorEDA) to generate a maximum of 12 unique sentences, which were created through RS(randomly swapping), RD(randomly deleting), RI(randomly inserting) or SR (synonym replacing) techniques. The resulting - somewhat broken - sentences were then subjected to a machine translation pipeline for auto-correction and to ensure diversity in language. In the original phase, our dataset was augmented to contain 20,993 samples. The dataset provided in this repository is a selection from the original dataset, chosen to balance out the distribution of labels.

## Caution

This is an experimental dataset that focuses on constructing a domain-specific dataset with limited resources. When using this dataset, please note that only 1,843 samples (773 in the training set, 70 in the development set, and 1,000 samples in the test set) are either human-generated or human-verified. We cannot guarantee the grammatical accuracy or label precision of each entry.

During our initial qualitative analysis, which involved randomly selecting 1,000 samples, we found that 87% of the samples were acceptable(i.e., the hypothesis was human-understandable), 93% when including the human-generated samples. However, there were instances where significant information was missing or the sentences were grammatically incomprehensible.

The model's performance on our human-verified test dataset, after fine-tuning [KoElectra v3](https://huggingface.co/monologg/koelectra-base-v3-discriminator) using a combination of our LEAP data, [KorNLI](https://huggingface.co/datasets/kor_nli), and [KLUE NLI](https://huggingface.co/datasets/klue/viewer/nli/train) dataset, resulted in an accuracy score of 0.944.

It is still highly recommended to conduct further human analysis when utilizing this dataset to ensure its suitability for specific use cases.


## Acknowledgments

The authors thank the [Legal Informatics and Forensic Science (LIFS) institute](https://lifs.hallym.ac.kr/) at Hallym University and its researchers for their indispensable help in creating the data. 

## Citations

Publication in process.

```bibtex
@article{williams-etal-2020-anlizing,
  title = "Lessons learned building a legal inference dataset",
  author = "Sungmi Park and Joshua I. James",
  journal = "Artificial Intelligence and Law",
  year = "2023",
}
```