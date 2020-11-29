# 한국어 노이즈 추가 (noising-korean)
한국어 문서에 노이즈를 추가하는 것을 도와주는 소스 코드입니다.


## 실행 방법
```
python run_nosing_text.py --input_dir <input_directory> --output_dir <output_directory> \
--noise_mode spliting_noise --noise_prob 0.1 --prefix noised_ --delimiter \n --num_cores 16
```
- input_dir: 입력 파일들이 위치한 폴더의 경로입니다.
- output_dir: 출력 파일들이 저장될 폴더의 경로입니다(파일이름:<prefix>+<input_filename>).
- noise_mode: 노이즈를 생성하는 방법을 설정합니다(default:'spliting_noise', support mode: ['spliting_noise', 'vowel_noise', 'phonological_noise']). 
- noise_prob: 노이즈가 생성될 확률을 결정합니다(default:0.1, 단위 문서에 대해 적용됩니다).
- prefix: 출력 파일 앞에 붙는 접두사입니다(default:'').
- delimiter: 입력 파일에서 문서의 단위화를 위한 분리 구분자입니다(default:'', 문장이 \n 으로 분리되어 있을 경우 \n으로 설정하여 단위 문장에 대해 노이즈를 추가합니다).
- num_cores: 멀티 프로세싱을 위한 cpu core의 개수를 설정합니다(default:사용가능한 모든 core 개수).
  


## 노이즈 생성 방법
노이즈를 생성하는 방법은 총 3가지가 구현되어 있습니다.

**[spliting_noise]** 자모 분리(alphabet separation)에 의한 노이즈 추가 방법. 글자의 자음과 모음을 분리합니다. 단, 가독성을 위해 종성이 없으며 중성이  'ㅘ', 'ㅙ', 'ㅚ', 'ㅛ', 'ㅜ', 'ㅝ', 'ㅞ', 'ㅟ', 'ㅠ', 'ㅡ', 'ㅢ', 'ㅗ' 가 아닐 경우 실행합니다(예: 안녕하세요 > 안녕ㅎㅏㅅㅔ요)

**[vowel_noise]** 모음 변형에 의한 노이즈 추가 방법. 글자의 모음을 변형시킵니다. 단, 가독성을 위해 종성이 없으며 중성이 'ㅏ', 'ㅑ', 'ㅓ', 'ㅕ', 'ㅗ', 'ㅛ', 'ㅜ', 'ㅠ' 일 경우 실행합니다(예: 안녕하세요 > 안녕햐세오).

**[phonological_noise]** 음운변화에 의한 노이즈 추가 방법. 발음을 바탕으로 단어를 변형시킵니다(너무 닮았다 > 너무 달맜다).


**변형 예시**
```
[original]  행복한 가정은 모두가 닮았지만, 불행한 가정은 모두 저마다의 이유로 불행하다.

[spliting_noise, prob=1] 행복한 갸정은 묘듀갸 닮았지만, 불행한 갸정은 묘듀 져먀댜의 이우료 불행햐댜.

[vowel_noise, prob=1] 행복한 ㄱㅏ정은 모두ㄱㅏ 닮았ㅈㅣ만, 불행한 ㄱㅏ정은 모두 ㅈㅓㅁㅏㄷㅏ의 ㅇㅣ유로 불행ㅎㅏㄷㅏ.

[phonological_noise, prob=1] 행복한 가정은 모두가 달맜지만, 불행한 가정은 모두 저마다의 이유로 불행하다.
```

## 기타
- 3번의 방법은 현재 비음화, 유음화, 구개음화, 연음 등을 구현하고 있습니다(누락된 규칙이 있을 수 있으니, 발견 시 피드백 주시면 감사하겠습니다).
- prob는 변형 가능한 글자들에 대해서 해당 확률만큼 확률적으로 실행됩니다(prob가 1이라고 해서 모든 텍스트가 변경되는 것이 아닙니다).
- 두 개 이상의 방법을 사용할 경우(쉼표로 구분), 한 단위 텍스트에서 두 개의 방법이 사용되는 것이 아니라 각 단위 텍스트마다 랜덤하게 방법을 결정하여 실행합니다.
- 'phonological_noise'은 국립국어원 표준국어대사전을 이용하여 만든 데이터(data/word_pron_pair.txt)를 기반으로 변형을 실행합니다.
- 'phonological_noise'은 미리 정의된 사전 데이터 방식으로 변형을 하기에, 추후 개선이 필요합니다.
