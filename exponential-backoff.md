# Exponential Backoff

Exponential backoff is an easy to avoid [rate limit errors](https://docs.voyageai.com/docs/rate-limits) or potential limitations with infrastructure capacity or scheduling.

Exponential backoff means performing a short sleep when an error is hit, then retrying the unsuccessful request. If the request is still unsuccessful, the sleep length is increased and the process is repeated. This continues until the request is successful or until a maximum number of retries is reached.

The following code snippet demonstrates how to implement exponential backoff (with a little bit of jitter) in Python using the [tenacity package](https://tenacity.readthedocs.io/en/latest/).

```
import voyageai

vo = voyageai.Client()

from tenacity import (  
	retry,  
	stop_after_attempt,  
	wait_random_exponential,  
)  # for exponential backoff

@retry(wait=wait_random_exponential(multiplier=1, max=60), stop=stop_after_attempt(6))  
def embed_with_backoff(**kwargs):  
    return vo.embed(**kwargs)

documents = [
	"The Mediterranean diet emphasizes fish, olive oil, and vegetables, believed to reduce chronic diseases.",
	"Photosynthesis in plants converts light energy into glucose and produces essential oxygen.",
	"20th-century innovations, from radios to smartphones, centered on electronic advancements.",
	"Rivers provide water, irrigation, and habitat for aquatic species, vital for ecosystems.",
	"Apple’s conference call to discuss fourth fiscal quarter results and business updates is scheduled for Thursday, November 2, 2023 at 2:00 p.m. PT / 5:00 p.m. ET.",
	"Shakespeare's works, like 'Hamlet' and 'A Midsummer Night's Dream,' endure in literature."
]

embed_with_backoff(texts=documents, model="voyage-large-2", input_type="document")
```
