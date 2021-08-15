# ML로 번역하고 감정 분석하기

이전 강의에서 noun phrase 추출하는 기초 NLP 작업을 하기 위해 ML behind-the-scenes을 포함한 라이브러리인, `TextBlob`으로 기본적인 봇을 만드는 방식을 배웠습니다. 컴퓨터 언어학에서 다른 중요한 도전은 구두나 다른 언어로 문장을 정확하게 _translation_ 하는 것입니다.

## [강의 전 퀴즈](https://white-water-09ec41f0f.azurestaticapps.net/quiz/35/)

번역은 천여 개 언어와 각자 많이 다른 문법 규칙이 있다는 사실에 의해서 합쳐진 매우 어려운 문제입니다. 한 접근 방식은 영어처럼, 한 언어의 형식적인 문법 규칙을 비-언어 종속 구조로 변환하고, 다른 언어로 변환하면서 번역합니다. 이 접근 방식은 다음 단계로 진행된다는 점을 의미합니다:

1. **Identification**. nouns, verbs 등으로 입력하는 언어의 단어를 식별하거나 태그를 답니다.
2. **Create translation**. 타겟 언어 포맷의 각 단어로 바로 번역합니다.

### 예시 문장, 영어를 아일랜드어로

'영어'에서, _I feel happy_ 문장은 순서대로 3개 단어가 이루어집니다:

- **subject** (I)
- **verb** (feel)
- **adjective** (happy)

그러나, '아일랜드' 언어에서, 같은 문장은 매우 다른 문법적인 구조를 가지고 있습니다 - "*happy*" 또는 "*sad*" 같은 감정이 *다가오는* 것으로 표현되었습니다.

아일랜드어로  `Tá athas orm`인 영어 표현은 `I feel happy`입니다. *문자 그대로* 번역하면 `Happy is upon me`입니다.

영어로 번역하는 아일랜드 사람은 `I feel happy`라고 말하며, `Happy is upon me`는 아니라고 합니다,
그 이유는 단어와 문장 구조가 다르다면, 문장의 의미를 이해하는 게 달라진다고 생각했기 때문입니다.

아일랜드어 문장의 형식적인 순서는 이렇습니다:

- **verb** (Tá or is)
- **adjective** (athas, or happy)
- **subject** (orm, or upon me)

## 번역

전문적이지 않은 변역 프로그램은 문장 구조를 무시하고, 단어만 번역할 수 있습니다.

✅ 만약 성인이 되고나서 두번째 (혹은 세 번보다 더 많은) 언어를 배웠다면, 번역문을 말할 때, 모국어로 생각하고 머리 속 개념으로 두번째 언어를 번역했을 것입니다. 전문적이지 않은 번역 프로그램이 하는 일과 유사합니다. 유창하게 하려면 이 단계를 넘기는 것이 중요합니다!

전문적이지 않은 번역은 나쁘고 (그리고 때떄로 명쾌한) 잘 못된 번역으로 될 수 있습니다: 아일랜드어에서 `Mise bhraitheann athas`는 문자 그대로 `I feel happy`로 번역합니다. (문자 그대로) `me feel happy`를 의미하지만 올바른 아일랜드어 문장은 아닙니다. 영어와 아일랜드어는 섬에 가깝게 붙어서 사용하는 언어지만, 다른 문법 구조로 인해서 매우 다른 언어가 되었습니다.

> [this one](https://www.youtube.com/watch?v=mRIaLSdRMMs)에서 아일랜드 언어 전통에 관련한 약간의 비디오를 시청할 수 있습니다.

### 머신러닝 접근 방식

아직, natural language processing 에 형식적인 룰 접근하는 방식에 대하여 배웠습니다. 다른 접근 방식은 단어의 의미를 무시하고, _대신 머신러닝으로 패턴을 감지_ 하는 방식입니다. 만약 원본과 타겟 언어 모두에 많은 텍스트 (a *corpus*) 또는 몇 텍스트 (*corpora*)를 가진다면 번역할 수 있습니다.

예시로, 1813년에 Jane Austen이 쓴 잘 알려진 영어 소설, *Pride and Prejudice* 케이스를 고려해봅니다. 만약 영어로 된 책과 *French*로 되어 번역한 책을 참고해보면, _idiomatically_ 가 다르게 번역된 구문을 찾을 수 있습니다. 몇 분에 할 수 있습니다.

예시로, `I have no money` 같은 영어 구문을 불어로 그대로 번역할 때, `Je n'ai pas de monnaie`로 될 수 있습니다. "Monnaie"는 'money' 와 'monnaie'가 동의어가 아니므로, 까다로운 불어 'false cognate' 입니다.  돈이 없다는 ('monnaie' 의미인 'loose change' 보다) 의미를 더 잘 전달할 수 있기 때문에, 사람이 만들 수 있는 더 좋은 번역은 `Je n'ai pas d'argent` 일 것 입니다.

![monnaie](../images/monnaie.png)

> Image by [Jen Looper](https://twitter.com/jenlooper)

만약 ML 모델에 이를 만들 충분한 수동 번역이 되는 경우, 언어에 다 능숙한 사람이 이전에 번역한 텍스트에서 공통적인 패턴을 식별하여 번역의 정확도를 향상시킬 수 있습니다.

### 연습 - 번역

`TextBlob`을 사용해서 문장을 번역합니다. **Pride and Prejudice**의 유명한 첫 라인으로 시도합니다:

```python
from textblob import TextBlob

blob = TextBlob(
    "It is a truth universally acknowledged, that a single man in possession of a good fortune, must be in want of a wife!"
)
print(blob.translate(to="fr"))

```

`TextBlob`은 번역 작업을 은근히 잘합니다: "C'est une vérité universellement reconnue, qu'un homme célibataire en possession d'une bonne fortune doit avoir besoin d'une femme!". 

사실은, V. Leconte and Ch. Pressoir 책의 1932년 불어 번역보다, TextBlob의 번역이 더 정확하다고 주장할 수 있습니다:

"C'est une vérité universelle qu'un celibataire pourvu d'une belle fortune doit avoir envie de se marier, et, si peu que l'on sache de son sentiment à cet egard, lorsqu'il arrive dans une nouvelle residence, cette idée est si bien fixée dans l'esprit de ses voisins qu'ils le considèrent sur-le-champ comme la propriété légitime de l'une ou l'autre de leurs filles."

이 케이스는, 'clarity'하게 원저자가 불필요한 단어를 넣는 수작업 번역보다 ML에서 제안하는 번역이 더 좋게 작업합니다.

> 어떤가요? TextBlob이 더 좋게 번역되는 이유는 무엇인가요? 음, 그 뒤에는, 수작업으로 가장 적당한 문자열을 예측한 수백만 문장을 파싱할 수 있는 정교한, AI Google 번역을 사용하고 있습니다. 여기에 수동으로 진행되지 않고, `blob.translate`를 사용하려면 인터넷 연결이 필요합니다.

✅ 몇 문장을 더 시도해봅니다. ML이나 수작업 번역 중에, 어떤 게 좋나요? 어떤 케이스에서 말이죠?

## 감정 분석

머신러닝이 감정 분석을 더 잘 작업하는 것은 다른 영역입니다. 감정에 대한 비-ML 방식은 'positive' 와 'negative'인 단어와 구문를 식별합니다. 그러면, 새로운 텍스트의 조각이 주어졌을 때, 긍정, 부정과 중립 단어의 모든 값을 계산하여 전체적인 감정을 식별합니다.

이 방식은 Marvin 작업에서 봤던 것처럼 쉽게 속았습니다 - 문장 `Great, that was a wonderful waste of time, I'm glad we are lost on this dark road` 는 빈정되고 부정적인 감정 문장이지만, 간단한 알고리즘은 긍정적으로 'great', 'wonderful', 'glad'를, 부정적으로는 'waste', 'lost'와 'dark'를 파악했습니다. 충돌되는 단어로 전체적인 감정이 흔들립니다.

✅ 잠시 시간을 내어 사람이 이야기할 때 어떻게 풍자를 전달하는지 멈추고 생각해봅니다. 목소리 톤 조절하는 게 큰 룰입니다. 목소리가 의미를 어떻게 전달하는지 발견하는 다른 방식으로  "Well, that film was awesome" 구문을 말해봅니다.

### ML 접근 방식

ML 접근 방식은 텍스트의 부정과 긍정적인 본문을 수동으로 수집합니다 - 트윗, 또는 영화 리뷰, 또는 사람이 점수를 주고 의견을 작성할 수 있는 모든 것. NLP 기술이 의견과 점수를 적용할 수 있는 순간에, 패턴이 드러납니다 (예시. 긍정적인 영화 리뷰는 'Oscar worthy' 구문을 부정적인 영화 리뷰보다 더 많이 사용하는 경향이 있으며 긍정적인 레스토랑 리뷰는 'disgusting'보다 'gourmet'라고 더 말하는 경향이 있습니다). 

> ⚖️ **Example**: 만약 정치인 사무실에서 일하고 있으며 새 법을 검토하면, 선거권자들은 새로운 법에 대한 서포팅하거나 반대하는 메일을 사무실로 보낼 수 있습니다. 이메일을 읽고  *for*와 *against* 기준으로 2 파일로 분류하는 일을 한다고 가정합니다. 만약 많은 메일을 받으면, 모든 것을 읽으려고 시도하다가 숨막힐 수 있습니다. 만약 봇이 모든 것을 읽고 이해해서 이메일이 어느 파일에 속하는지 알려줄 수 있다면, 좋을 수 있나요?
> 
> 하나의 방식은 머신러닝을 사용해서 이루어내는 것입니다. 이메일의 *against* 일부와 *for* 일부를 모델에 훈련합니다. 모델은 구문과 단어를 찬성과 반대 측면으로 연관하려는 경향이 있지만, *모든 컨텐츠를 이해하지 못하고*, 오직 특정 단어와 패턴이 *against* 또는 *for* 이메일에 나타날 것 같다는 것만 알게됩니다. 모델을 훈련하지 않은 몇 이메일로 테스트할 수 있으며, 같은 결말이 나왔다는 것을 볼 수 있습니다. 그렇게, 모델의 정확도를 만족하게 된다면, 각자 읽을 필요없이 이후 이메일을 처리할 수 있습니다.

✅ 이 프로세스가 이전 강의에서 사용했던 것처럼 들리나요?

## 연습 - 감정적인 문장

감정은 -1 에서 1로 *polarity* 측정하며, 가장 부정적인 문장은 -1 으로 의미하고, 그리고 1은 가장 긍정적입니다. 감정은 objectivity (0) 와 subjectivity (1)를 0 - 1 점으로 측정하기도 합니다.

Jane Austen의 *Pride and Prejudice*를 다르게 봅니다. 텍스트는 [Project Gutenberg](https://www.gutenberg.org/files/1342/1342-h/1342-h.htm)에서 존재합니다. 샘플은 책 처음과 마지막 문장의 감정을 분석하고 감정 polarity와 subjectivity/objectivity 점수를 출력하는 짧은 프로그램이며 다음과 같이 보여집니다.

다음 작업에서 (설명한) `TextBlob` 라이브러리로 `sentiment`를 (감정 계산기를 작성하지 않아도) 탐지합니다.

```python
from textblob import TextBlob

quote1 = """It is a truth universally acknowledged, that a single man in possession of a good fortune, must be in want of a wife."""

quote2 = """Darcy, as well as Elizabeth, really loved them; and they were both ever sensible of the warmest gratitude towards the persons who, by bringing her into Derbyshire, had been the means of uniting them."""

sentiment1 = TextBlob(quote1).sentiment
sentiment2 = TextBlob(quote2).sentiment

print(quote1 + " has a sentiment of " + str(sentiment1))
print(quote2 + " has a sentiment of " + str(sentiment2))
```

다음 출력을 보게 됩니다:

```output
It is a truth universally acknowledged, that a single man in possession of a good fortune, must be in want # of a wife. has a sentiment of Sentiment(polarity=0.20952380952380953, subjectivity=0.27142857142857146)

Darcy, as well as Elizabeth, really loved them; and they were
     both ever sensible of the warmest gratitude towards the persons
      who, by bringing her into Derbyshire, had been the means of
      uniting them. has a sentiment of Sentiment(polarity=0.7, subjectivity=0.8)
```

## 도전 - 감정 polarity 확인

만약 *Pride and Prejudice*가 절대적 부정적인 내용보다 긍정적인 문장이 더 있다면, 작업은 감정 polarity로 탐지합니다. 이 작업에서, 1 or -1 polarity 점수가 각자 절대적으로 positive 하거나 negative 하다고 생각할 수 있습니다.

**단계:**

1. .txt 파일로 이루어진 Project Gutenberg의 [copy of Pride and Prejudice](https://www.gutenberg.org/files/1342/1342-h/1342-h.htm)를 내려받습니다. 파일의 시작과 끝에 있는 메타데이터를 제거해서, 원본 텍스트만 남깁니다
2. Python으로 파일을 열고 문자열로 컨텐츠를 풉니다 
3. 책의 문자열로 TextBlob을 만듭니다
4. 책의 각 문장을 반복해서 분석합니다
   1. 만약 polarity가 1 또는 -1이면 문장을 배열이나 positive 또는 negative 메시지 리스트에 저장합니다
5. 마지막으로, (각자) 모든 긍정적인 문장과 부정적인 문장, 각 수를 출력합니다

여기에 샘플 [solution](../solution/notebook.ipynb)이 있습니다.

✅ 지식 점검

1. 감정은 문장에서 사용된 단어를 기반하지만, 코드는 단어를 *이해하나요*? 
2. 감정 polarity가 정확하다고 생각하거나, 다시 말하면, 점수에 *동의* 하나요?
   1. 특히나, 다음 문장이 절대적으로 **긍정** polarity라는 점을 동의하거나 거부하나요?
      * “What an excellent father you have, girls!” said she, when the door was shut.
      * “Your examination of Mr. Darcy is over, I presume,” said Miss Bingley; “and pray what is the result?” “I am perfectly convinced by it that Mr. Darcy has no defect.
      * How wonderfully these sort of things occur!
      * I have the greatest dislike in the world to that sort of thing.
      * Charlotte is an excellent manager, I dare say.
      * “This is delightful indeed!
      * I am so happy!
      * Your idea of the ponies is delightful.
   2. 다음 3개 문장은 절대적으로 긍정 문장이지만, 자세히 읽으면, 긍정 문장이 아닙니다. 왜 감정이 긍정 문장으로 생각하도록 분석되었나요?
      * Happy shall I be, when his stay at Netherfield is over!” “I wish I could say anything to comfort you,” replied Elizabeth; “but it is wholly out of my power.
      * If I could but see you as happy!
      * Our distress, my dear Lizzy, is very great.
   3. 다음 문장을 절대적인 **부정** polarity로 동의하거나 거부하나요?
      - Everybody is disgusted with his pride.
      - “I should like to know how he behaves among strangers.” “You shall hear then—but prepare yourself for something very dreadful.
      - The pause was to Elizabeth’s feelings dreadful.
      - It would be dreadful!

✅ 모든 Jane Austen의 팬은 자주 자신의 책으로 English Regency 사회의 말도 안되는 측면을 비판하는 것을 이해합니다. *Pride and Prejudice*의 주요 캐릭터인 Elizabeth Bennett은, (작성자 같은) 예민한 소셜 옵저버이며 그녀의 언어는 가끔 뉘앙스가 미묘합니다. 심지어 Mr. Darcy (이야기에 흥미로운 것을 좋아하는 자)도 Elizabeth의 장난스럽고 놀리는 언어를 주목합니다: "I have had the pleasure of your acquaintance long enough to know that you find great enjoyment in occasionally professing opinions which in fact are not your own."

---

## 🚀 도전

사용자 입력으로 다른 features를 추출해서 Marvin을 더 좋게 만들 수 있나요?

## [강의 후 퀴즈](https://white-water-09ec41f0f.azurestaticapps.net/quiz/36/)

## 검토 & 자기주도 학습

텍스트에서 감정을 추출하는 많은 방식이 있습니다. 이 기술로 사용할 수 있는 비지니스 애플리케이션을 생각해봅니다. 어떻게 틀릴 수 있는지도 생각해봅니다. [Azure Text Analysis](https://docs.microsoft.com/azure/cognitive-services/Text-Analytics/how-tos/text-analytics-how-to-sentiment-analysis?tabs=version-3-1?WT.mc_id=academic-15963-cxa) 같이 감정 분석을 하는 정교한 enterprise-ready 시스템에 대하여 읽어봅니다. Pride and Prejudice 일부 문장에서 미묘한 차이를 감지할 수 있는지 테스트 합니다.

## 과제 

[Poetic license](../assignment.md)
