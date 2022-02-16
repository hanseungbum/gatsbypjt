---
title: 클레이튼 수업 7
subtitle: Bapp 설계
date: 2022-02-08
slug: 실제로 만들 Bapp 설계
author: han
rating: 5
coverImage: 
---
## MarKet Bapp 설계
1 기능
    ㄱ. Klip 지갑 주소 가져오기
    ㄴ. KLAY 잔고 조회
    ㄷ. NFT 조회(유저, 마켓)
    ㄹ. NFT 발행
    ㅁ. NFT 판매(마켓에 전송)
    ㅂ. NFT 구매

2 구현 내용
    ㄱ. Klip 지갑 주소 가져오기 : Klip API
    ㄴ. KLAY 잔고 조회 : caver-js(getbalance) 
    ㄷ. NFT 조회(유저, 마켓) : caver-js
    ㄹ. NFT 발행 : Klip API
    ㅁ. NFT 판매(마켓에 전송) : Klip API
    ㅂ. NFT 구매 : Klip API
    ㅅ. 브라우저에서는 QR, 모바일에서는 바로 카카오톡 klip 지갑 앱으로 연동
