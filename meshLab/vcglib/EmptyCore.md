# Class Template: EmptyCore

The most interesting class template in vcglib should be EmptyCore. In fact, there 
are four such class templates, lying in vcglib/vcg/simplex/vertex/component.h, 
vcglib/vcg/simplex/face/component.h, vcglib/vcg/simplex/edge/component.h and 
vcglib/vcg/simplex/tetrahedron/component.h.

EmptyCore 之所以精彩，是因为可以通过模板类 EmptyCore 给不同类增加相同的 section 完成类似的 
aspect。EmptyCore 的模板参数是其父类，当父类即模板参数不同时就给不同的类增加了相同的 
section(截面)，而 EmptyCore 实现的其实就是截面的功能。