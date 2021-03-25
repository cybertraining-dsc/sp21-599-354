:o2: permissions are denied
:o2: notebook should be checked in

Link to the Google Colab: <https://colab.research.google.com/drive/1htOCu7Mpi5SYxJcedv9fZcPAFVv-xCMp#scrollTo=c3o_k-adkOy4>

The accuracy of the runs were fairly consistent throughout all combinations of batch size and epochs. The lowest accuracy run I had was 91.5% with a batch size of 4 and 5 epochs, while the highest accuracy run was 92.6%. 92.6% was achieved with three batch/epoch combinations: 32/50, 65/20, and 128/20. To test these combinations further, I ran each of them three times again. None of them reached 92.6% accuracy again, but the 128/20 combination consistently got 92.4% accuracy for all three runs. Although there were certainly fluctuations, the general trend was that for more epochs and larger batches, the accuracy slightly improved.
