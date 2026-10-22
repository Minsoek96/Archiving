# intervgym

모의면접 연습 서비스. 질문을 고르고, 녹화하며 답하고, 답변 영상·대본·자세·음성 분석과 AI 피드백을 받는다. 이전 프로젝트 [인자강](injagang.md)의 면접 기능을 새 구조(Next.js App Router, FSD)로 옮기면서 다시 설계했다.

- 스택: Next.js(App Router), React 19, TypeScript, Tailwind, zustand, react-query, MediaPipe, Jest + RTL
- AI: OpenAI(gpt-6-luna) AI 피드백, Groq Whisper 답변 받아쓰기·질문 생성

## 기록

| 글 | 한 줄 요약 |
| --- | --- |
| [AI 피드백 프롬프트: 외워서 맞히던 평가를 처음 보는 문항으로 다시 쟀다](intervgym/feedback-prompt-eval.md) | 평가 오염을 찾아내고 판정 오류 16/296 → 8/296, 페르소나 프롬프트보다 2배 빠름 |
| [질문 읽기 TTS: 380MB 모델을 93MB로](intervgym/tts-model-size.md) | 공개 TTS 모델을 부분 양자화해 브라우저에서 합성, 원본과 SNR 40~46dB |
