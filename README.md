# Can Manus transcribe the way Singapore talks?

A hands-on test of Manus speech-to-text on real mixed Mandarin-English speech, benchmarked against a specialist speech tool. By Miao Gao, September 2026.

I gave Manus a real two-minute recording of a speaker switching between Mandarin and English, then checked every line against the official transcript. Here is what worked, what broke, and how I would describe the feature to users without overclaiming.

## Why speech-to-text

Good speech-to-text frees people from typing. Meetings, driving, and classrooms are all places where talking is easier than typing, and sometimes the only option.

But many people in Asia speak two languages and switch between them in the same sentence. In my experience, general-purpose AI tools still struggle with this. That gap affects a very large group of users: consumers across Asia and anyone who is fluent in two languages. A tool that handles mixed-language speech well has real business potential.

As a former conference interpreter, I have worked with this kind of speech for years, so I wanted to see how well an AI agent handles it today.

## How I tested it

- **Audio:** a 2-minute sample from SEAME, a research dataset of Mandarin-English code-switching speech from Singapore and Malaysia. The speaker talks casually about old English pop songs and karaoke.
- **Answer key:** the official transcript from the dataset. I did not give it to Manus.
- **Prompts:** (1) a word-for-word transcript, (2) the same with a glossary of names, (3) a bilingual summary, (4) a self-check where Manus lists its least confident spots.
- **Runs:** I ran the full set twice to see if results stayed the same.
- **Tool:** Manus used its built-in speech-to-text utility. It said it could not tell which underlying model the utility uses.
- **Benchmark:** I ran the same clip twice through a specialist speech-to-text tool, ElevenLabs, the second time with key terms, and scored it the same way.

## Scorecard (Manus)

| What I checked | Run 1 | Run 2 | Verdict |
|---|---|---|---|
| English names and song titles (9 terms) | 9 of 9 | 9 of 9 | Strong |
| Switch points, e.g. “go back 回去之前的” | Correct | Correct | Strong |
| Local particles 咯 · 啦 · 嘛 | about 5 errors | about 5 errors | Weak |
| Local wording 不大 · 多数 | 4 errors | 3 errors | Weak |
| Topic guesses (wrong word that fits the topic) | 2, one flagged as unsure | 2, stated as fact | Risk |
| Who said what | Not separated | Not separated | Missing |
| Self-check honesty | Good | Weaker | Mixed |

Counts are from my own line-by-line check. The first line was cut off by the clip and is not scored.

## What I found

### 1. English inside Mandarin is handled well

Every band name and song title came out right: Backstreet Boys, Celine Dion, Air Supply, Michael Learns to Rock, “As Long As You Love Me”, “Drowning”. Switch points like “很 rock 的” and “跟 colleague 这样去咯” were also correct. The glossary only fixed spelling, such as Red Box to Redbox.

### 2. It turns Singapore Mandarin into textbook Mandarin

| Said | Transcribed |
|---|---|
| 不大能接受 | 不太能接受 |
| 多数都是 | 都是，都是 |
| 这样咯 | 这样 |

The meaning mostly survives, but the local voice does not. Particles like 咯 and 啦 carry tone in Singapore speech, and they were dropped or swapped several times in both runs.

### 3. It guesses from the topic, and so does the specialist tool

| Said | Transcribed |
|---|---|
| english 的算 | English 的 song |
| certain 的啦 | 《Scent of a Woman》 |

The speaker is talking about music, so Manus filled in words that fit the topic instead of what was said. It turned the Mandarin word 算 into the English word “song”, which is a wrong-language error that only happens in mixed speech. It also invented a song title. In run 1, Manus listed the title as one of its least confident spots. In run 2 it stated it as fact. I listened to both spots myself, and the official transcript is right.

ElevenLabs made the same two mistakes at the same two spots, writing “song” and a different made-up title, 《Sometime》. So this is a problem with mixed-language speech in general, not with one tool. It is the most important finding, because these errors look plausible and a reader would not question them.

### 4. Results change between runs

The same audio gave different words each time. Run 2 fixed 多数是去 Redbox 啦 in the last line but broke 旧歌这样啦, which run 1 had right. For anything important, one run is not enough.

### 5. It does not separate speakers unless asked

The quieter interviewer's questions, like “你还有去 karaoke 吗？”, were merged into the main speaker's text with no labels. A reader could think the speaker asked those questions. My prompts did not ask for speaker labels, so this is a gap I still need to test.

## Benchmark: a specialist speech tool

To see whether these results are good or bad, I ran the same clip through ElevenLabs speech-to-text, a tool built only for transcription.

| What I checked | Manus | ElevenLabs |
|---|---|---|
| English names and song titles | All correct | All correct |
| Local wording 不大 | Changed to 不太 | Kept 不大 |
| Local particles 咯 · 啦 | about 5 errors | about 4 to 5 errors |
| Topic guesses | 2 per run | 2 per run, same spots |
| Interviewer's voice | Mixed into the speaker's text | Left out |
| Same result on each run | Varied | Almost identical |
| Warns about unsure spots | Yes, when asked | No |
| Summary and key points | Yes, in English and Chinese | Transcript only |

The specialist tool is more consistent and keeps more of the local wording. Manus does more with the result: it summarises, pulls out key points, and can say where it is unsure. The two tools failed in the same places, which tells me those spots are hard for current speech models, not a Manus-only weakness.

## How I would describe this to users

The job of product marketing here is to make the feature attractive without promising more than it does. The benchmark points to a clear position: Manus should not compete on raw transcription accuracy. Its strength is the whole workflow, from audio to transcript to summary to next steps, with a check on where it is unsure.

**Overclaim:** “Manus transcribes mixed English and Mandarin perfectly.”

**Accurate:** “Drop in a meeting where people switch between English and Mandarin. Manus gives you the transcript, a bilingual summary and the key points, and flags the parts worth a second listen.”

**Good for**

- Getting the gist of a mixed-language chat fast
- Catching names, brands and English terms
- Bilingual summaries of what was said

**Tips for better results**

- Add a glossary of names and terms
- Ask for speaker labels
- Ask Manus to flag unsure spots, then check them
- Double-check any word that fits the topic a little too neatly
- Run important files twice and compare

## Limits of this test

- One 2-minute clip with one main speaker and a casual topic. Business meetings may behave differently.
- The glossary run happened inside the same Manus task, so it may not be fully independent of the first run.
- Error counts are my own manual check against the official transcript, not an automated score.
- ElevenLabs was run in its web app with default settings. Its key terms are limited to 5 words, so one song title could not be added.

**Next tests:** a fresh-task glossary run, a speaker-label run, and my own recording of a real Singapore-style meeting.

---

Audio and reference transcript: SEAME free sample, Mandarin-English Code-Switching in South-East Asia (LDC2015S04), Linguistic Data Consortium. Only short quotes are shown here; the audio and full transcript are not included in this repository. Benchmark run with ElevenLabs speech-to-text, September 2026. Test design, checking and write-up by Miao Gao.
