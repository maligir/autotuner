# autotuner

This application is a simple audio autotuner software created using Python.

## Pitch Tracking

The YIN algorithm estimates the pitch from the time-domain signal through computing the autocorrelation function and then refining the result. PYIN extends this approach by applying a Hidden Markov Model to the outputs of the YIN algorithm. Given a monophonic audio signal, the length of the analysis frames frame_length, the distance between the frames hop_length, the sampling rate sr, and the minimum and the maximum viable frequencies fmin and fmax, we obtain the estimated pitch for each frame f0, the information whether the given frame was voiced or not voiced_flag, and the probability of each frame being voiced voiced_probabilities. We can use the information in the f0 vector to correct the pitch in the voiced frames as indicated by the voiced_flag. Alternatively, we can rely on the fact that f0 has not-a-number (NaN) values at the unvoiced frames.

```python
f0, voiced_flag, voiced_probabilities = librosa.pyin(audio,
                                                     frame_length=frame_length,
                                                     hop_length=hop_length,
                                                     sr=sr,
                                                     fmin=fmin,
                                                     fmax=fmax)
```

## Shift Pitch

Overlap-and-add (OLA) techniques allow us to change the time duration of a signal without changing its pitch (i.e., perform a time-scale modification, TSM) by dividing the signal into a series of overlapping frames and then reassembling those frames but with a different distance between the frames.
Resampling allows us to change the duration and the pitch of the signal together (see my tutorial on the variable speed replay effect which uses this approach). A combination of an OLA-technique with resampling allows us to change the pitch of a signal without changing its duration. OLA is used to counteract the inherent time-scale modification outcome of the resampling part.
