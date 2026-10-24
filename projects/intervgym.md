# intervgym

모의면접 연습 서비스. 질문을 고르고, 녹화하며 답하고, 답변 영상·대본·자세·음성 분석과 AI 피드백을 받는다. 이전 프로젝트 [인자강](injagang.md)의 면접 기능을 새 구조(Next.js App Router, FSD)로 옮기면서 다시 설계했다.

- 스택: Next.js(App Router), React 19, TypeScript, Tailwind, zustand, react-query, MediaPipe, Jest + RTL
- AI: OpenAI(gpt-6-luna) AI 피드백·질문 생성, Groq Whisper 답변 받아쓰기

## 기록

| 글 | 한 줄 요약 |
| --- | --- |
| [AI 피드백 프롬프트: 외워서 맞히던 평가를 처음 보는 문항으로 다시 쟀다](intervgym/feedback-prompt-eval.md) | 평가 오염을 찾아내고 판정 오류 16/296 → 8/296, 페르소나 프롬프트보다 2배 빠름 |
| [질문 읽기 TTS: 380MB 모델을 65MB로](intervgym/tts-model-size.md) | 공개 TTS 모델을 브라우저에서 합성. int8 계산이 브라우저에서 5배 느려, 가중치만 int8로 담는 65MB 판으로 바꿔 문장당 1.4~3.5초 |
| [면접 질문 생성: 무료 모델을 두고 유료 모델을 고른 이유](intervgym/question-generation-model.md) | 12개 직무 블라인드 평가에서 Luna 전부 1위, 규칙 위반 직무당 0.7 대 4~5개. 프롬프트로 안 되는 조건은 코드로 |
| [아무도 정하지 않은 import 순서를 에이전트들이 지키고 있었다](intervgym/implicit-import-order.md) | 규칙이 없었는데 층 둘 이상을 import 하는 파일 86%(옮겨 온 파일 100%)가 원래 정리 방식을 지켰다. 판단이 필요 없는 부분만 lint로 굳혔다 |
