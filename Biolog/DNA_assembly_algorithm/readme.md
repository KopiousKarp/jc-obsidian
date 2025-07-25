The readme for the auto assembly algorithm for Biolog


The general idea: use the massive number of sequences and raw trace files that we have stored and organized to train a transformer model that tokenizes the basecalled reads with the raw traces and predicts the bases. 

pre-processing: use [tracy an open source base calling program](https://www.gear-genomics.com/docs/tracy/) to call the bases and output the rough sequence. This will also output the trace values for each basecall 


post-processing: Use Blast+ to keep a fast database of expected sequences at the ready, blast the resulting sequence and match the miscalled bases to the blast+ sequence


Architecture: Still researching this, but I think transformer architecture is going to be the best bet. However I wouldn't mind testing the dataloaders and training loops on a simpler model, such as a heavy neural network.

Environment: Naturally, I'm gonna build this all in docker. Tracy has a docker container made already but I think it might be better to build from a lightweight python/anaconda container as Tracy has a [bioconda package](https://anaconda.org/bioconda/tracy). 
Actually I think I have a few approaches to consider 
- [Tracy dockerfile](https://hub.docker.com/r/geargenomics/tracy)
- [python/anaconda docker]((https://hub.docker.com/r/continuumio/anaconda3)
- [blast+ dockerfile](https://hub.docker.com/r/ncbi/blast)
- combination of all 3 with docker-compose

