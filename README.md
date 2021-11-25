# Neural-Architecture-Search

Neural Architectural search (NAS)
Code and report



RC 10 3 tanh;NC 20 3 relu;RC 10 3 swish;FL swish; <br>

NC 32 3 relu;RC 64 3 relu;RC 128 3 relu;FL sigmoid; <br>

NC 16 3 relu;RC 32 3 tanh;RC 64 3 tanh;FL sigmoid; <br>

RC 32 3 relu;NC 64 3 sigmoid;RC 128 3 relu;FL sigmoid; <br>

NC 16 3 relu;RC 32 3 relu;RC 64 3 relu;FL sigmoid; <br>

RC 16 3 tanh;NC 32 3 relu;RC 64 3 tanh;FL swish; <br>

RC 10 3 relu;NC 20 3 sigmoid;RC 10 3 relu;FL sigmoid; <br>

RC 20 3 relu;NC 10 3 tanh;RC 20 3 relu;FL sigmoid; <br>

NC 32 3 tanh;RC 64 3 gelu;RC 128 3 gelu;FL sigmoid; <br>
