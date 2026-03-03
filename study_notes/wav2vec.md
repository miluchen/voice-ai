## wav2vec 2.0: Overview

- **Learns from audio without transcripts**
- Understands speech by hiding parts of the audio and forcing the model to guess what was there
- Trained directly on raw waveforms

---

### Step-by-Step Intuition

1. **Input raw audio**  
   Feed in a waveform; the model converts it into low-level acoustic features using a convolutional encoder.  
   → *Learns its own spectrogram, instead of using a precomputed STFT.*

2. **Mask time steps**  
   Randomly hides chunks of time (or removes parts of the latent representation).

3. **Predict missing parts**  
   The model uses surrounding context to predict what the hidden chunk should be.  
   → *Predicts which discrete audio unit was likely there.*

4. **After pretraining**  
   Remove the masking objective, add a small ASR head, and fine-tune on labeled speech.  
   → *Requires much less labeled data and achieves better accuracy.*

---

### Why This Works Well for Speech

- Speech is highly redundant
- Speech is temporally structured
- Speech is locally predictable

---

### Key Idea

`wav2vec` is a *representation learner*—it learns acoustic features by solving a hard self-supervised prediction task, rather than by using manually designed signal processing features like MFCCs or Mel spectrograms.  
The learned feature space performs extremely well for ASR.

---

### Shortcomings

- Assumes full context (not streaming-friendly)
- Uses future information during pretraining
- Chunking can destabilize representation
- Large models are memory-heavy