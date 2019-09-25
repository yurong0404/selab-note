* Attention layer<br>
video: https://youtu.be/oaV_Fv5DwUM<br>
> Attention用在model能夠根據input自行調整input token的權重，做法為多加一層layer訓練給予input的權重。<br>
例如：要訓練輸入句子，輸出sentiment為positive或negative，input有"I like it"、"I dislike it"、"It is awful"，
到底該給第幾個token比較高的權重？固定給某個位置的token比較高的權重很不靈活，應該根據input本身自行決定第幾個token擁有高權重，
因此多給一個attention layer輸入input tokens，輸出input tokens的權重，用該權重與input tokens相乘，才往後面的layer繼續運算，
這做法才夠彈性去預測該句子是positive或negative。
