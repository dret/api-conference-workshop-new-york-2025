## LLM Context Documents

You can load these documents into hour favorite LLM in order to teach it how to parse API Story documents into a (reasonably) valid OpenAPI document.

1. Load the `ruleset`, `context`, and `quality` documents into the LLM and execute to "teach" the engine about api stories
2. load your API Story and ask the LLM to convert it from API Story to OpenAPI
3. when it is done, tell the LLM to run the quality checklist to review the results in more detail

