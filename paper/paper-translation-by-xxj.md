# 摘要

​		多边形网格是一种高效表示三维形体的方法。它可以明确地表示形体的表面信息和拓扑结构，并且通过减少形体的不均匀性来表示大而平滑的表面、几何形体以及复杂的特征。三维形体的这种不均匀性和不规则性，使得用神经网络（结合了卷积和池化操作）分析多边形网格难以进行。在这篇文章中，我们利用了网格的独特属性，并使用meshCNN方法来直接分析三维形体，meshCNN方法是一种为三角形网格模型特别设计的卷积神经网络。与传统的CNN类似，MeshCNN通过消除模型内部的连接使得卷积和池化层可以在网格模型的边上进行操作。卷积是在边以及边所在的两个三角形上的另外四条边上应用的，池化是通过能够维持物体表面拓扑结构的边坍缩操作来实现的，通过上述方法，为后续的卷积层生成新的连通网格。MeshCNN需要去学会让那条边坍缩，那么就可以设定一个有目的性的学习过程了，神经网络在该过程中发现并且放大重要的特征同时舍弃那些冗余的。我们在多个应用了三维形体的学习任务中，证明了该方法的高效性。

# 引言

​	三维形体不仅在计算机图形学中占据了非常重要的位置，在其他相关领域，比如计算机视觉、几何计算，也非常重要。我们身边的形体，特别是描述自然物体的形体，一般都是由连续的面组成的。

​	出于方便计算和加速数据处理的原因，许多对三维形体的离散型近似方法被引入，这些表示形体的方法被许多领域所使用。其中最受欢迎的，是多边形网格表示法，又称网格法，简而言之就是使用许多二维多边形的组合来近似出三维空间中的表面。网格法，为形体提供了一个高效的、不均匀的表示方法。一方面，对于大而简单的表面我们只需要很少的多边形就可以表示了，另一方面，表示的灵活性是的它能够支持更加高级的操作，比如对于那些几何上比较复杂的形体特征可以进行高质量的重构，以及图形表示的构建。另一个很好的特征是网格表示法天生提供了连接信息。这使得对表面的表示非常全面。

​	这些优势相比于另一个流行的表示方法--点云 是很显然的。尽管点云表示法很简单而且和一般的数据获取技术（扫描）直接相关，但在需要 较高的质量 或者 保存尖锐特征时，效果就不理想了。

​	近些年，卷积神经网络在图片上的使用在诸多领域（比如分类和语义分割）效果拔群，

