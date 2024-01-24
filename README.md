# Transformer_Seg

# 資料收集規範 Data Collection

```text
Data:.
├─Face
├─Voice
├─Original
└─Image
    ├─S1252001
    ├─S1252002
    ├─S1252003
    ├─S1252004
    └─S1252005
		...
```
# 系統架構 Architecture
```mermaid
%%{
  init: {
		"theme": "base",
    "themeVariables": {
      "darkMode": "true", 
      "background":"#202020",
      "primaryColor": "#233B48",
      "primaryTextColor": "#fff",
      "primaryBorderColor": "#ADC6CD",
      "lineColor": "#ADC6CD",
      "secondaryColor": "#ADC6CD",
      "tertiaryColor": "#1C1C1C"
    }
  }
}%%
flowchart TD
    SS(Semantic Segmentation)-->FRL(Facial Region Localization)
    FRL-->FR(Facial Recognition)
    FR-->IL(Identity Locking)
    IL-->VPV(Voiceprint Verification)
    VPV-->VC(Verification Complete)

```



```mermaid
%%{
  init: {
		"theme": "base",
    "themeVariables": {
      "darkMode": "true", 
      "background":"#202020",
      "primaryColor": "#233B48",
      "primaryTextColor": "#fff",
      "primaryBorderColor": "#ADC6CD",
      "lineColor": "#ADC6CD",
      "secondaryColor": "#ADC6CD",
      "tertiaryColor": "#1C1C1C"
    }
  }
}%%
flowchart LR
input(user)-->OS(Operation Interface)
OS --> Image(Camera)
OS --> Voice(Microphone)
OS .-> Keyboard(Keyboard)
Image --> Face(Face)
Voice --> Speak(Voice)
Keyboard .->LOG(User Name with password)
subgraph Raspberry Pi
Face --> |YOLOv8| FFE(Facial Feature Extraction)
Speak --> |Deep Speaker|VFE(Voiceprint Feature Extraction)
FFE --> FF(Feature Fusion)
VFE --> FF
end
FF --> NAS(NAS)
LOG .-> PV
PV(Password Vaildation) .-> |Incremental Learning| NAS
NAS --> IM(Identity Matching)

```
![image](https://github.com/wry87c/TEEP2023/assets/147070503/61d2c6fd-ae41-403c-83cd-e070a7ed63a3)


# 整合 Combination
```mermaid
%%{
  init: {
		"theme": "base",
    "themeVariables": {
      "darkMode": "true", 
      "background":"#202020",
      "primaryColor": "#233B48",
      "primaryTextColor": "#fff",
      "primaryBorderColor": "#ADC6CD",
      "lineColor": "#ADC6CD",
      "secondaryColor": "#ADC6CD",
      "tertiaryColor": "#1C1C1C"
    }
  }
}%%
flowchart LR
input(face image)
layer1(Conv2D<br/>ReLU<br/>Banch Norm<br/>Max Pooling 2D)
layer2(Conv2D<br/>ReLU<br/>Banch Norm<br/>Max Pooling 2D)
layer3(FC<br/>ReLU<br/>Banch Norm)
layer4(Weighted<br/>sum)
output1(FC<br/>Softmax)
output2(Bimodal embedding)
MFCC(MFCC <br/>Speech input)
M1(Conv2D<br/>ReLU<br/>Banch Norm)
M2(Conv2D<br/>ReLU<br/>Banch Norm)
GRU3(GRU<br/>layers x3)
GRU7(GRU<br/>layers x7)
output3(FC Softmax)
input-->layer1-->layer2-->layer3-->layer4 -->|Weighted sum<br/>output|output1 & output2


MFCC-->M1-->M2-->GRU3 & GRU7
GRU3-->layer4
GRU7-->output3

subgraph Bimodal identification output
output1 & output2
end

subgraph Speech Recognition output
output3
end
```

