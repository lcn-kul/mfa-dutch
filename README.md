# Montreal Forced Aligner -- Dutch

Applying Montreal Forced Aligner to Dutch. [`my_corpus`](./my_corpus) contains the input corpus and [`my_corpus_aligned`](./my_corpus_aligned) contains the result.

**Note:** example-0001 and example-0002 are generated by whisper, example-0003 is manually annotated.

## Installation

https://montreal-forced-aligner.readthedocs.io/en/latest/installation.html#general-installation

```
conda create -n aligner -c conda-forge montreal-forced-aligner
conda activate aligner
```

## Download Pretrained Models

```bash
# Usage: mfa model download [OPTIONS] [TYPE] [MODEL_NAME]
mfa model download --ignore_cache acoustic dutch_cv
mfa model download --ignore_cache dictionary dutch_cv
```

## Train G2P Model
```bash
# Usage: mfa train_g2p [OPTIONS] DICTIONARY_PATH OUTPUT_MODEL_PATH
mfa train_g2p --clean -j 16 dutch_cv ~/Documents/MFA/btamm/g2p/dutch_cv.zip
# mfa train_g2p --clean --phonetisaurus -j 16 dutch_cv ~/Documents/MFA/btamm/g2p/dutch_cv_phonetisaurus.zip --alignment_separator "°"
```

The arg --alignment_separator "°" avoids the following error, I haven't tested if it works properly though...

```
The symbol ";" is reserved for "alignment_separator", but is found in the graphemes or phonemes of your dictionary.
Please re-run and specify another symbol that is not used in your dictionary with the "--alignment_separator" flag.
```

## Validate Model on Corpus

This finds OOVs in the corpus.

```bash
# Usage: mfa validate [OPTIONS] CORPUS_DIRECTORY DICTIONARY_PATH
mfa validate --clean --ignore_acoustics ./my_corpus dutch_cv
```

## Generate OOV Pronunciations

```bash
# Usage: mfa g2p [OPTIONS] INPUT_PATH G2P_MODEL_PATH OUTPUT_PATH
mfa g2p --clean ~/Documents/MFA/my_corpus/oovs_found_dutch_cv.txt ~/Documents/MFA/btamm/g2p/dutch_cv.zip ~/Documents/MFA/my_corpus/g2p_oovs.txt --dictionary_path dutch_cv
```

## Add Pronunciations To Dictionary

```bash
# Usage: mfa model add_words [OPTIONS] DICTIONARY_PATH NEW_PRONUNCIATIONS_PATH
mfa model add_words --clean dutch_cv ~/Documents/MFA/my_corpus/g2p_oovs.txt
```


## Align Corpus

```bash
# Usage: mfa align [OPTIONS] CORPUS_DIRECTORY DICTIONARY_PATH ACOUSTIC_MODEL_PATH OUTPUT_DIRECTORY
mfa align --clean --output_format json ./my_corpus dutch_cv dutch_cv ./my_corpus_aligned
```
