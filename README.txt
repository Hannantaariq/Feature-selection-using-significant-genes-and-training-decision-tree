Performing feature selection on the expression dataset using the significant genes as recognized before. 

> load("eset.RData")
> expression.df<-as.data.frame(t(exprs(eset)))
> class.vector <-as.factor(c(rep("DOX", 6), rep("noDox", 6)))
> library(siggenes)
> cl<-c(rep(0,6),rep(1,6))
> sam.out<-sam(t(expression.df),cl, rand=142)

We're doing 924 complete permutations
and randomly select 100 of them.

> findDelta(sam.out,fdr=0.05)
The threshold seems to be at 
     Delta Called      FDR
5 0.516047   2449 0.050020
6 0.516048   2448 0.049912
> sam.sum<-summary(sam.out,0.516047)
> library(RWeka)
> expression.df.fsub<-expression.df[,sam.sum@row.sig.genes]
> print(dim(expression.df.fsub))
[1]   12 2449
> print(dim(expression.df))
[1]    12 40083
> j48.model.fsub<-J48(class.vector~.,expression.df.fsub)
> print(j48.model.fsub)
J48 pruned tree
------------------

1568932_at <= 4.220443: DOX (6.0)
1568932_at > 4.220443: noDox (6.0)

Number of Leaves  : 	2

Size of the tree : 	3

