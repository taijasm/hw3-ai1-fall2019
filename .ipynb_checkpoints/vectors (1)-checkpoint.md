From Manning et.al's information retrieval book:

> We assign to each term in a document a weight for that term, that depends on the number of occurrences of the term in the document. We would like to compute a score between a query term t and a document d, based on the weight of t in d. The simplest approach is to assign the weight to be equal to the number of occurrences of term t in document d. This weighting scheme is referred to as term frequency (TF).

The simplest such view of a document is the **Bag of Words** view, in which we do not care about the ordering of the words in the document. We just care about the number of occurences of a term in the document.

> Thus, the document “Mary is quicker than John” is, in this view, identical to the document “John is quicker than Mary”.

Not all words are equally important. 

> Sometimes, some extremely common words which would appear to be of little value in helping select documents matching a user need are excluded  from the vocabulary entirely. These words are called **stop words**. 

These are words like "a", "the", etc,  and can often be obtained by looking at the most frequent words in a set of documents. Here we will provide you with a list of such words to remove from your list of all words found in a vocabulary.

You will compute a **vocabulary** by taking a union of all the words in all the documents, and then removing the stop words. For each document then, we will create a vocabulary sized list, in which each index corresponds to a particular word. The value at that index corresponds to the frequency of the words in that document.

> The representa- tion of a set of documents as vectors in a common vector space is known as the *vector space model* and is fundamental to a host of information retrieval op- erations ranging from scoring documents on a query, document classification and document clustering.  
>
> We denote by *V*⃗ (*d*) the vector derived from document *d*, with one com- ponent in the vector for each dictionary term. Unless otherwise specified, the reader may assume that the components are computed using the tf-idf weighting scheme, although the particular weighting scheme is immaterial to the discussion that follows. The set of documents in a collection then may be viewed as a set of vectors in a vector space, in which there is one axis for each term. This representation loses the relative ordering of the terms in each document; 

![](https://www.dropbox.com/s/tbm9d4a46t2p91m/Screenshot%202019-12-11%2014.01.08.png?dl=1)

> How do we quantify the similarity between two documents in this vector space? A first attempt might consider the magnitude of the vector difference between two document vectors. This measure suffers from a drawback: two documents with very similar content can have a significant vector difference simply because one is much longer than the other. Thus the relative distribu- tions of terms may be identical in the two documents, but the absolute term frequencies of one may be far larger. 
>
> To compensate for the effect of document length, the standard way of quantifying the similarity between two documents *d*1 and *d*2 is to compute the *cosine similarity* of their vector representations *V*⃗ (*d*1) and *V*⃗ (*d*2) 
>
> sim(*d*1,*d*2)= *V*⃗(*d*1)·*V*⃗(*d*2), |*V*⃗ (*d*1)||*V*⃗ (*d*2)| 
>
> where the numerator represents the *dot product* (also known as the *inner prod- uct*) of the vectors *V*⃗ (*d*1) and *V*⃗ (*d*2), while the denominator is the product of their *Euclidean lengths*. The dot product ⃗*x* · ⃗*y* of two vectors is defined as 
>
> *M*⃗􏰑
>  ∑*i*=1 *x**i**y**i*. Let *V*(*d*) denote the document vector for *d*, with *M* components 
>
> *V*⃗1(*d*) . . . *V*⃗*M*(*d*). The Euclidean length of *d* is defined to be ∑*M* *V*⃗ 2(*d*). *i*=1 *i* 
>
> The effect of the denominator of Equation (6.10) is thus to *length-normalize* the vectors *V*⃗ (*d*1) and *V*⃗ (*d*2) to unit vectors ⃗*v*(*d*1) = *V*⃗ (*d*1)/|*V*⃗ (*d*1)| and ⃗*v*(*d*2) = *V*⃗ (*d*2)/|*V*⃗ (*d*2)|. We can then rewrite (6.10) as (6.11) sim(*d*1, *d*2) = ⃗*v*(*d*1) ·⃗*v*(*d*2). Thus, (6.11) can be viewed as the dot product of the normalized versions of the two document vectors. This measure is the cosine of the angle *θ* between the two vectors, shown in Figure 6.10. What use is the similarity measure sim(*d*1, *d*2)? Given a document *d* (potentially one of the *d**i* in the collection), consider searching for the documents in the collection most similar to *d*. Such a search is useful in a system where a user may identify a document and seek others like it – a feature available in the results lists of search engines as a *more like this* feature. We reduce the problem of finding the document(s) most similar to *d* to that of finding the *d**i* with the highest dot products (sim values)⃗*v*(*d*)·⃗*v*(*d**i*). Wecoulddothisbycomputingthedotproductsbetween ⃗*v*(*d*) and each of⃗*v*(*d*1),...,⃗*v*(*d**N*), then picking off the highest resulting sim values. 

STEMMING?



![](https://www.dropbox.com/s/yppp9mh61czmmiv/Screenshot%202019-12-11%2013.59.50.png?dl=1)

>here is a far more compelling reason to represent documents as vectors: we can also view a *query* as a vector. Consider the query *q* = jealous gossip. This query turns into the unit vector ⃗*v*(*q*) = (0, 0.707, 0.707) on the three coordinates of Figures 6.12 and 6.13. The key idea now: to assign to each document *d* a score equal to the dot product ⃗*v*(*q*) · ⃗*v*(*d*). In the example of Figure 6.13, *Wuthering Heights* is the top-scoring docu- ment for this query with a score of 0.509, with *Pride and Prejudice* a distant second with a score of 0.085, and *Sense and Sensibility* last with a score of 0.074. This simple example is somewhat misleading: the number of dimen- sions in practice will be far larger than three: it will equal the vocabulary size *M*. 
>
>To summarize, by viewing a query as a “bag of words”, we are able to treat it as a very short document. As a consequence, we can use the cosine similarity between the query vector and a document vector as a measure of the score of the document for that query. The resulting scores can then be used to select the top-scoring documents for a query. Thus we have 
>
>score(*q*, *d*) = *V*⃗ (*q*) · *V*⃗ (*d*) . |*V*⃗ (*q*)||*V*⃗ (*d*)| 
>
>A document may have a high cosine score for a query even if it does not contain all query terms.  Computing the cosine similarities between the query vector and each doc- ument vector in the collection, sorting the resulting scores and selecting the top *K* documents can be expensive — a single similarity computation can entail a dot product in tens of thousands of dimensions, demanding tens of thousands of arithmetic operations.  

![](https://www.dropbox.com/s/j9avemp5f7zfdsp/Screenshot%202019-12-11%2014.01.54.png?dl=1)

![](https://www.dropbox.com/s/1ggkr73x3mrgz2e/Screenshot%202019-12-11%2014.02.47.png?dl=1)