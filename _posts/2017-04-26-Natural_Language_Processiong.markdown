---
title:  "NLP(Natural Language Processing)"
categories: [Tech]
tags: [Tech]
---

**1. Process of NLP**    
:  사용자로부터 입력된 발화(Text)는 NLP Server에서...    
   - 발화를 분류하고, 기 정의한 키워드로 변환한다.    
   - 변환한 키워드 간의 가중치 점수를 비교하여 Domain과 MainAction을 결정한다.    
   - 정해진 MainAction에 따라 실제 서비스를 호출하고 결과를 반환한다.    
**NLP** : Natural Langauge Processing  
**NLU** : Natural Language Understanding  
**DM** : Data Mining  
**NLG** : Natural Language Generator  


![Process of NLP](https://parkmh04.github.io//images/processofNLU.png)    

**2. NLP Preprocessing(사전 준비)**    
 - 사용자가 입력한 발화(Text, Vocie To Text)를 System에서 처리가 가능하도록 Normalizing한다.(이모티콘 등 일반적인 Charset으로 처리하지 못하는 Uni-code 문자열이 들어올 경우가 있을 수 있음.)    
 - 특수문자 제거, 빈칸 제거, Date format 통일 등 데이터의 형태를 정리하기도 한다.  
  ex. ) 광 화문에 가려면 어떻;게 해야해???  > 광화문에가려면어떻게해야해
  
**3. POS Tagger(형태소 분석)**    
  - Normalize 된 사용자 발화를 형태소 분석을 통해서 형태소와 품사를 구분한다.  
   ex. )  [ Entity Name: 광화문/NC, Text: 광화문,  Entity Name: 에/JCS, Text: 가, Entity Name: 가/A, Text: 가 Entity Name: 면/PA, Text: 면....]    
  - 언어학적 특징이 반영되기 때문에 서비스할 언어마다 POS Tagging 방식, 성능의 차이를 보일 수 있다.     
     [오픈소스 POS Tagger(한국어)](http://jammun.blogspot.kr/2014/07/pos-tagger.html)    
     [스탠포드 POS Tagger(영어) 설치하기](http://www.citrus-translation.com/stanford-pos-tagger-on-localhost/)
      
**4. Refined Entity By Dictonary(Entity 정련)**    
-  Pos Tagger에서 분류한 Entity를 사전에 정의한 Rule 기반으로 정련한다.  
  ex. ) 저녘 > 저녁, 원걸 > 원더걸스...     

**5. Classify Domain & Action**    
-  정련된 Entity들을 처리하여 대상 Domain(분야)과 수행할 Action(기능)을 결정한다.  
**Domain** : 날씨, 검색, 영화,  리모컨,  채팅...    
**Action** : 오늘 날씨 검색, 현재 상영중인 영화 조회, 에어콘 켜기...
      
- Domain 후보 추출  
  영화 : web_search, entity_title, entity_person, email, reserve_movie...
    
- Rule에 의한 정렬(Entity가 해당되는 Rule)  
  (영화, web_search), (영화, entity_person), (영화, emali), (영화, entity_title)...
    
- 가중치 계산  
   web_search = 103.12321, entity_person = 82.12121, emali = 77.18851, entity_title =100.9911...
    
- Entity 별로 반복하여 가중치가 높은 Entity의 Domain과 Action 결정
        
**6. Finalize NE(Named Entity)**
    
- 결정된 Domain, Action에 해당되는 기능 실행    
  ex) “weather_search_today” > 오늘 날씨 검색 
        “movie_ search_title” > 영화 제목 검색  
		“chatting” > 채팅