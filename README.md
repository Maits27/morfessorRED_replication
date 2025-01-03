# morfessorRED_replication: A trial to replicate morfessoRED 
This is an attempt to replicate the results given by [morfessoRED](https://github.com/sjtodd/morfessoRED) repository that adds reduplication templates to Morfessor 2.0

## Requirements
* Install Morfessor 2.0 (https://github.com/aalto-speech/morfessor)
* Copy the .py files in the original repo to the source location of Morfessor 2.0, overwriting source files (`baseline.py`, `cmd.py`, `io.py`, `evaluation.py`, `representations.py`) to try the extended version


## Data used
Since I don't know anything about Maori language and the data used in the morfessoRED paper are not avaliable, to try the code given in the paper different words with and without reduplication are used from this sources:
* [Reduplication in Maori](https://lisatravis2012.wordpress.com/2017/10/19/reduplication-in-maori/#:~:text=Maori%20also%20uses%20reduplication%20as,will%20further%20extend%20the%20meaning.)
* [Maori, Wikipedia](https://mi.wikipedia.org/wiki/M%C4%81ori)
* [An Analysis of Maori Reduplication](http://roa.rutgers.edu/files/133-0496/133-0496-0-1.PDF)
* [High frequency word lists, The Reo Maori](https://tereomaori.tki.org.nz/Teacher-tools/Te-Whakaipurangi-Rauemi/High-frequency-word-lists#:~:text=Two%20common%20prefixes%20in%20M%C4%81ori,suffix%20and%20the%20nominal%20suffix.&text=Prefixes%20and%20suffixes%20add%20elements%20of%20meaning%20to%20a%20word.)
* And the paper itself: [link](https://aclanthology.org/2022.sigmorphon-1.2/)

The `train.txt` and `test.txt` files used to train and test the models are avaliable in the folder `data`. Since there isn't much data avaliable, there are not many words.

## Experiments
The results given in the [original paper](https://aclanthology.org/2022.sigmorphon-1.2/) are not possible to replicate since the data is not avaliable. However, a test to see if the templates help to identify reduplication cases with a small sample can be done. 

### Experiments with Morfessor 2.0 without changes
To create the binary model (`model.bin`):

```
python morfessor-train -s model.bin -S training_style.txt train.txt
```

Once we have the binary model (`model.bin`) we test it:

```
python morfessor-segment -l model.bin -o test_output.txt test.txt
```
### Experiments with extended Morfessor 2.0 

Training and testing without templates. We try this again to see if the results variate in comparison to the Morfessor 2.0 without changes:

```
python morfessor -t train.txt -S training_style.txt -T test.txt -o test_output.txt --atom-separator " " --strip-atom-sep  --regexsplit "(?<=(\S)) (?=\1)" --randseed 123 --finish-threshold 0.00005 --progressbar --Lred-minbase-weight 2 --Rred-minbase-weight 4 --delay-nonedge-red --backup
```


Training and testing with all templates:
```
python morfessor -t train.txt -S training_output.txt -T test.txt -o test_output.txt --atom-separator " " --strip-atom-sep --regexsplit "(?<=(\S)) (?=\1)" --randseed 123 --finish-threshold 0.00005 --progressbar --Lred-minbase-weight 2 --Rred-minbase-weight 4 --delay-nonedge-red --backup --left-reduplication --right-reduplication --skippable-full-red
```




