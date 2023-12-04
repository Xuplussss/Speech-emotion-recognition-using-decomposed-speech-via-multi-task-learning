# Speech-emotion-recognition-using-decomposed-speech-via-multi-task-learning
!["our proposed system frameworks"](https://github.com/Xuplussss/Speech-emotion-recognition-using-decomposed-speech-via-multi-task-learning/blob/main/systemframeworks.png?raw=true)

語音分解模型  pre-train  大多都在SpeechSplit資料夾下進行

    虛擬環境 pip install -r requirements.txt

    ./SpeechSplit/create_gender.ipynb 根據資料標記性別 產生assets/gender.pickle

    ./SpeechSplit/make_spct_f0.py 產生對應音檔的spectrogram & pitch contour 分別存入 assets/spmel & assets/raptf0

    ./SpeechSplit/metadata.ipynb 產生訓練語音分解資料 assets/spmel/train.pkl   
    -----------------------------------------------------------------------------------這裡上面都是資料前處理  若沒有換資料庫都不需要重新執行
    ./SpeechSplit/python main.py  開始train模型 
        training arguments 注意 
        SpeechSplit有兩種訓練版本
            SpeechSplit/solver.py L59 L60 改變要訓練語音分解模型還是single encoder
            L157 ~ L185 模型訓練& Loss 計算  用註解表達那些模型需要保留那些程式碼  SORRY
            L237 238 
            L273~286
        

語音分解模型預訓練 end ---------------------------------------------

最後模型訓練(speech emotion recognition model)   interspeech21_emotion

    ./interspeech21_emotion/make_spct_f0.py 產生./interspeech21_emotion/spmel & ./interspeech21_emotion/raptf0  (也可以直接從 SpeechSplit/assets底下拿過來)

    bash run.sh 訓練模型 
        訓練不同種類的模型 SER only, final emotion model, single encoder
        L468 472 478 選擇 要setup哪種wav2vec2 model  註解解釋如何選擇
        L640 選擇 compute_metrics  (大部分訓練用  包含WER) OR  compute_SERonly_metrics (SER only 模型專用)

        訓練結果  wandb 網站看 目前我是用offline 訓練完畢之後 console會給一段 上傳logs的指令
        之前的訓練結果 學長創完wandb 帳號後 應該可以用以下網址看到全部LOG
        https://wandb.ai/ja5p3r/huggingface?workspace=user-ja5p3r

        其中取名  single_enc_01F 就是 single encoder(不做語音分解的結果) 的 第01F當作test set的結果
        01F_SER SER only model 的結果
        01F_test 是 final model 包含著4個loss speech decomposition, multitasklearning 的大集合
        tmp_01F 是 只有 SER 跟 ASR 的結果

## Reference
This package provides training code for the audio-visual emotion recognition paper. If you use this codebase in your experiments please cite: 

```
@inproceedings{hsu2023speech,
  title={Speech Emotion Recognition using Decomposed Speech via Multi-task Learning},
  author={Hsu, Jia Hao and Wu, Chung Hsien and Wei, Yu Hung},
  booktitle={Proceedings of the Annual Conference of the International Speech Communication Association, INTERSPEECH},
  volume={2023},
  pages={4553--4557},
  year={2023}
}
```
