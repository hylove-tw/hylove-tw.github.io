# Hylove Sample Site

## Build with
### Vue 3 + Vite

This template should help get you started developing with Vue 3 in Vite. The template uses Vue 3 `<script setup>` SFCs, check out the [script setup docs](https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup) to learn more.

### Recommended IDE Setup

- [VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=johnsoncodehk.volar)

## Quick Start
1. set environment veriables or use `.env` at source root
```shell
# 使用mock auth機制取代登入驗證
VITE_MOCK_AUTH=true
VITE_MOCK_USERNAME=user
VITE_MOCK_PASSWORD=user

# 設定API host路徑
VITE_APP_ENDPOINT=https://your.api.endpoint/api/v1/
```
2. install dependency via npm
```shell
npm install
```
3. run dev environment
```shell
npm run dev
```


## Build dist for production
```shell
npm run build
```

## API Spec
All api endpoints use same request body like following json data. 
```javascript
"beforeBrainData": {
      "Good Signal Quality(0-100)": [int], 
      "Attention": [int], 
      "Meditation": [int], 
      "Delta": [int], 
      "Theta": [int], 
      "Low Alpha": [int], 
      "High Alpha": [int], 
      "Low Beta": [int], 
      "High Beta": [int], 
      "Low Gamma": [int], 
      "High Gamma": [int], 
    },
    "afterBrainData": {
      "Good Signal Quality(0-100)": [int], 
      "Attention": [int], 
      "Meditation": [int], 
      "Delta": [int], 
      "Theta": [int], 
      "Low Alpha": [int], 
      "High Alpha": [int], 
      "Low Beta": [int], 
      "High Beta": [int], 
      "Low Gamma": [int], 
      "High Gamma": [int], 
    }
```
### Basic raw data
基本特徵數值與折線圖資料
- Method: POST
- Path: /analysis/brain_features
- response:
```
[
{
"name":"attention",
"data":[
{
"name":"前測",
"avg":float,
"amplitude":float,
"series":[[int, int], ...]
},
{
"name":"後測",
"avg":float,
"amplitude":float,
"series":[[int, int], ...]
}
]
},
{
"name":"meditation",
"data":[
{
"name":"前測",
"avg":float,
"amplitude":float,
"series":[[int, int], ...]
},
{
"name":"後測",
"avg":float,
"amplitude":float,
"series":[[int, int], ...]
}
]
},
...
{
"name":"low gamma",
"data":[
{
"name":"前測",
"avg":float,
"amplitude":float,
"series":[[int, int], ...]
},
{
"name":"後測",
"avg":float,
"amplitude":float,
"series":[[int, int], ...]
}
]
}
```
### Compared data
前後測數值比較
- Method: POST
- Path: /analysis/compared_features
- response:
```
[
{
"name":"attention",
"comparedData":{
"avg":{
"before":float,
"after":float,
"beforeAfter":float,
"afterBefore":float
},
"amplitude":{
"before":float,
"after":float
},
"amplitudeAvg":{
"before":float,
"after":float
}
}
},
...
{
"name":"highGamma",
"comparedData":{
"avg":{
"before":float,
"after":float,
"beforeAfter":float,
"afterBefore":float
},
"amplitude":{
"before":float,
"after":float
},
"amplitudeAvg":{
"before":float,
"after":float
}
}
}
]
```
