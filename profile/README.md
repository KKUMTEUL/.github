## 우리 아이 꿈의 틀을 잡아주는 맞춤형 성향 진단 및 도서 추천 서비스, 꿈틀
> LG U+ 유레카 종합 프로젝트 우수상 수상 <br>
> 개발기간: 2024.10.15 ~ 2024.11.03 (3주)

## Intro
1. 자녀 성향 진단
2. 맞춤형 도서 추천
3. 선착순 이벤트

## 기능
1. 메인 (도서 추천) <br>
콘텐츠 기반 추천과 협업 필터링 추천을 모두 사용하는 하이브리드 추천으로 Cold Start 문제 해결 및 새로운 아이템 추천 제약 완화
- 콘텐츠 기반 필터링
  - 사용자의 선호 장르와 선호 주제어, 좋아요 한 도서 장르와 주제어, 도서와 자녀의 MBTI를 기반으로 유사도 점수를 부여하여 콘텐츠 기반 필터링 점수 계산
- 협업 필터링
  - 사용자의 나이, 성별, MBTI에 따라 유사도 점수를 부여한 후 유사 사용자가 좋아요 한 도서 목록을 가져와 해당 도서에 프로필 유사도 점수를 합산
- 최종 점수
  - 콘텐츠 기반 필터링과 협업 필터링을 일정 가중치를 줘 합산한 후 최종 점수 순으로 상위 20개의 도서 중 5권 필터링
- 기본 추천 로직
  - 신규 사용자, 성향 히스토리가 없는 사용자, 필터링 된 추천 도서 수가 부족한 경우 도서 추천 연령대와 사용자 나이를 비교하여 가장 차이가 적은 도서를 반환함
  - 로그인 하지 않았을 경우 사용자 나이를 임의로 지정하여 도서 반환
- 위 추천 시스템은 Spring Batch를 사용하여 최근 7일 내 활동한 사용자를 기준으로 매일 자정에 추천 도서가 DB에 저장됨
<img src="https://github.com/user-attachments/assets/7ebf5ad3-8a68-4d87-8d19-94dfeda6fca4" width=50% height=50%>

<br>

2. 마이페이지
   - 유저 정보 조회, 수정, 자녀 프로필 관리
   - 자녀 성향 히스토리 상세 보기
<img src="https://github.com/user-attachments/assets/c1ba89a3-c763-4bc1-aacb-a70d7cba651a" width=50% height=50%>
<img src="https://github.com/user-attachments/assets/08ebe43b-114e-478d-a3b5-d47725d08aaa" width=40% height=40%>

<br>

3. 자녀 성향 진단 페이지
<img src="https://github.com/user-attachments/assets/5156a589-25df-46fd-bf24-8b26131f3c74" width=60% height=60%>

<br>

4. 자녀 성향 히스토리 페이지
   - 히스토리 삭제 시 실제 데이터를 삭제하지 않고 isDeleted 필드를 true로 논리적 삭제
   - Spring Batch를 사용하여 deletedAt이 한 달 전인 데이터 물리적 삭제 (Cascade, OrphanRemoval로 히스토리와 관계된 데이터도 물리적 삭제)

<br>

5. 도서 목록 조회 및 검색
<img src="https://github.com/user-attachments/assets/a00c77de-121c-4b65-9479-2f046b8629a6" width=60% height=60%>

<br>

6. 생성형 AI 기반 도서 MBTI 매핑
  - Hugging Face의 오픈 소스와 GPT API를 모두 사용해본 후 GPT API가 보다 높은 정확성을 보여 선택

<br>

7. 도서 상세 조회
<img src="https://github.com/user-attachments/assets/23cab6d9-2aea-447a-8f45-9a5d53d5690a" width=60% height=60%>
<img src="https://github.com/user-attachments/assets/dee1795f-5843-47b8-aeb8-2fa151ac5886" width=60% height=60%>

<br>

8. 도서 좋아요/싫어요에 따른 성향 변경
   - 좋아요/싫어요 선택 시 Kafka 메세지 전송
   - KafkaEventListener를 통해 Spring Batch로 좋아요/싫어요 한 책의 성향에 따라 아이 성향 점수 조정
   - Chunk 방식과 Tasklet 방식을 모두 사용하여 성능이 더 좋은 Tasklet 방식 선택

<br>

9. 이벤트 응모 시스템

<br>

10. 관리자 도서 CRUD 기능

<br>

## Trouble Shooting

---

## ERD
![image](https://github.com/user-attachments/assets/7873b2c1-7590-4de9-b5c3-5a03a7497236)

## SW 아키텍처
![image](https://github.com/user-attachments/assets/480578fa-f20b-4647-8d5b-71574c099a4b)

