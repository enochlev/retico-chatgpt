# retico-chatgpt

A module that uses OpenAI's GPT-3 to generate responses to user input incrementally.

## Example

```python
import retico_core
import retico_wav2vecasr
import retico_chatgpt
import dotenv

dotenv.load_dotenv()

INSTRUCTIONS = """You are an AI assistant that is an expert in alcoholic beverages.
You know about cocktails, wines, spirits and beers.
You can provide advice on drink menus, cocktail ingredients, how to make cocktails, and anything else related to alcoholic drinks.
If you are unable to provide an answer to a question, please respond with the phrase "I'm just a simple barman, I can't help with that."
Please aim to be as helpful, creative, and friendly as possible in all of your responses.
Do not use any external URLs in your answers. Do not refer to any blogs in your answers.
You are interacting with spoken words. Be very brief and do not format your responses.
Do not use dashes in lists, and do not use any other formatting.
Write out numbers as words, for example use "one" instead of "1".
"""

mic = retico_core.audio.MicrophoneModule()
asr = retico_wav2vecasr.Wav2VecASRModule(language="en")
chatgpt = retico_chatgpt.ChatGPTDialogueModule(system_prompt=INSTRUCTIONS)
printer = retico_core.debug.TextPrinterModule()

mic.subscribe(asr)
asr.subscribe(chatgpt)
chatgpt.subscribe(printer)

retico_core.network.run(mic)

print("Network is running")
input()

retico_core.network.stop(mic)
```

For this example, either the environment variable `OPENAI_API_KEY` needs to be set or a `.env` file needs to be present in the current working directory with the following content:

```bash
OPENAI_API_KEY=<your-openai-api-key>
```

