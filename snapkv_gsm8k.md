## GSM8K Performance Compare with SnapKV

| Strategy | Cache Budget (%) | Bert Precision | Bert Recall  | Bert F1 | Rouge 1 | Rouge 2 | Rouge L | Rouge Lsum | GPT4 Rouge | GPT4 Coherent | GPT4 Faithful | GPT4 Helpful | Avg Compression Ratio | Cache Mem (GB) | Prefill Toks Per Sec Top 10% | Decode Toks Per Sec Top 10% |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| SnapKV | 10 | 0.8288 | 0.7939 | 0.8107 | 0.2492 | 0.0793 | 0.1902 | 0.2300 | 1.0000 | 1.0000 | 1.0100 | 1.0000 | 0.3183 | 0.0288 | 1257.6962 | 18.4600 |
| SnapKV | 30 | 0.8336 | 0.8062 | 0.8194 | 0.2930 | 0.1019 | 0.2141 | 0.2628 | 1.0100 | 1.0000 | 1.0200 | 1.0000 | 0.0883 | 0.0328 | 1712.9594 | 22.8156 |
| SnapKV | 50 | 0.8337 | 0.8059 | 0.8192 | 0.2843 | 0.1017 | 0.2093 | 0.2558 | 1.0100 | 1.0000 | 1.0300 | 1.0000 | 0.0135 | 0.0367 | 1721.1882 | 22.8564 |
| SnapKV | 70 | 0.8330 | 0.8057 | 0.8188 | 0.2843 | 0.1020 | 0.2078 | 0.2539 | 1.0000 | 1.0000 | 1.0200 | 1.0000 | 0.0020 | 0.0417 | 1950.3116 | 23.3264 |
| SnapKV | 90 | 0.8330 | 0.8057 | 0.8188 | 0.2856 | 0.1029 | 0.2072 | 0.2550 | 1.0000 | 1.0000 | 1.0700 | 1.0000 | 0.0003 | 0.0456 | 2066.6882 | 24.0723 |
| HashEvict | 10 | 0.8312 | 0.8089 | 0.8197 | 0.1817 | 0.0429 | 0.1384 | 0.1657 | 1.5500 | 3.7100 | 2.5200 | 2.6500 | 0.8664 | 0.0032 | 1173.0333 | 15.7795 |
| HashEvict | 30 | 0.8431 | 0.8471 | 0.8449 | 0.2919 | 0.1099 | 0.2125 | 0.2618 | 2.5600 | 4.5100 | 3.7500 | 3.7100 | 0.7485 | 0.0072 | 2061.3773 | 18.6040 |
| HashEvict | 50 | 0.8468 | 0.8582 | 0.8524 | 0.3495 | 0.1556 | 0.2658 | 0.3188 | 2.9000 | 4.7400 | 4.2300 | 4.0900 | 0.6333 | 0.0113 | 2021.0625 | 18.5694 |
| HashEvict | 70 | 0.8470 | 0.8626 | 0.8547 | 0.3723 | 0.1741 | 0.2797 | 0.3418 | 2.9500 | 4.7800 | 4.3600 | 4.1300 | 0.4740 | 0.0164 | 2090.5373 | 18.7375 |
| HashEvict | 90 | 0.8485 | 0.8626 | 0.8554 | 0.3762 | 0.1768 | 0.2847 | 0.3439 | 3.0400 | 4.8000 | 4.4000 | 4.2000 | 0.3453 | 0.0205 | 1209.8592 | 15.4836 |

### Experiment Setup:
We setup SnapKV to compress the prompt only: the generated tokens do not cause any token eviction even if the cache budget is reached. We used a kernel size of 5 and observation window length of 16 (please see page 7 of https://arxiv.org/abs/2404.14469 for the effect of these hyperparameters). We set the compressed size of the KV cache of both SnapKV and HashEvict to be **cache budget * (median prompt length + allowed number of generated tokens)**. HashEvict will pick a token to evict if the cache size limit is reached. SnapKV compresses the prompt to fit inside the cache size limit, and during decoding it appends the generated token to kv cache and is allowed to exceed this limit. The experiments were conducted on Nvidia RTX-A6000 GPUs with 48GB of GPU Memory. 
