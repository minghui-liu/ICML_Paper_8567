## Multinews Performance Comparison with SnapKV

| Strategy | Cache Budget (%) | Bert Precision | Bert Recall  | Bert F1 | Rouge 1 | Rouge 2 | Rouge L | Rouge Lsum | Avg Compression Ratio | Cache Mem (GB) | Prefill Toks Per Sec Top 10% | Decode Toks Per Sec Top 10% |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| SnapKV | 10 | 0.7343 | 0.7936 | 0.7622 | 0.1858 | 0.0343 | 0.1154 | 0.1260 | 0.7052 | 0.0912 | 3955.6806 | 22.6462 |
| SnapKV | 30 | 0.7543 | 0.7992 | 0.7757 | 0.2191 | 0.0507 | 0.1334 | 0.1449 | 0.5139 | 0.1494 | 3713.2769 | 22.5308 |
| SnapKV | 50 | 0.7593 | 0.8023 | 0.7798 | 0.2326 | 0.0564 | 0.1383 | 0.1463 | 0.3599 | 0.2086 | 3848.1526 | 22.9063 |
| SnapKV | 70 | 0.7546 | 0.8012 | 0.7766 | 0.2186 | 0.0536 | 0.1310 | 0.1422 | 0.2474 | 0.2667 | 4332.6014 | 22.3639 |
| SnapKV | 90 | 0.7553 | 0.8020 | 0.7774 | 0.2269 | 0.0553 | 0.1353 | 0.1476 | 0.1695 | 0.3249 | 4888.0498 | 22.7895 |
| HashEvict | 10 | 0.8216 | 0.8300 | 0.8257 | 0.2995 | 0.0582 | 0.1497 | 0.1928 | 0.8810 | 0.0306 | 5137.7554 | 18.9564 |
| HashEvict | 30 | 0.8369 | 0.8474 | 0.8420 | 0.3565 | 0.0948 | 0.1736 | 0.2265 | 0.6649 | 0.0906 | 4968.8867 | 18.5221 |
| HashEvict | 50 | 0.8397 | 0.8526 | 0.8460 | 0.3687 | 0.1050 | 0.1795 | 0.2339 | 0.4788 | 0.1516 | 5057.2913 | 18.5573 |
| HashEvict | 70 | 0.8420 | 0.8548 | 0.8483 | 0.3772 | 0.1120 | 0.1837 | 0.2380 | 0.3420 | 0.2116 | 5081.9336 | 18.6746 |
| HashEvict | 90 | 0.8423 | 0.8563 | 0.8492 | 0.3821 | 0.1165 | 0.1865 | 0.2428 | 0.2388 | 0.2716 | 5161.6223 | 19.2068 |


### Experiment Setup:
We setup SnapKV to compress the prompt only: the generated tokens do not cause any token eviction even if the cache budget is reached. We used a kernel size of 5 and observation window length of 16 (please see page 7 of https://arxiv.org/abs/2404.14469 for the effect of these hyperparameters). We set the compressed size of the KV cache of both SnapKV and HashEvict to be **cache budget * (median prompt length + allowed number of generated tokens)**. HashEvict will pick a token to evict if the cache size limit is reached. SnapKV compresses the prompt to fit inside the cache size limit, and during decoding it appends the generated token to kv cache and is allowed to exceed this limit. The experiments were conducted on Nvidia RTX-A6000 GPUs with 48GB of GPU Memory. 

