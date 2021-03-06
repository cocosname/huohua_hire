
产品方案

一、问题背景 
在上课过程中，如果老师发现学生卡住了（或者学生告诉老师自己不能看到老师或无法操作课件），就会向技术支持提交一个网络状况工单，根据监课情况，工单原因包括：轻微抖动不影响上课和网络卡顿。为降低网络工单率，需要在工单提交时可以预测网络工单的关闭原因，这样就可以在老师提交工单时在后台直接关闭预测为“轻微抖动不影响上课”的工单，而仅放行“网络卡顿”的工单。

二、产品简介 
为解决上述问题，我们需要建立一个教师提交工单时立即响应的模型。当教师点击提交网络状况工单后即触发该模型，通过提交工单前一段时间内所记录的音频、视频及师生间课件控制的数据，可以得到该堂课属于正常课堂还是网络问题课堂的概率，由此判断该堂课是否需要标记为网络卡顿，进而触发工单的放行和关闭状态。

三、实现原理 
当前课堂有三种：正常课堂、轻微抖动不影响和网络卡顿，实际上前两种结果均可使得工单关闭，故在建模时去掉轻微抖动课堂的影响，只考虑正常课堂与网络卡顿课堂的区别，增大两类课堂特征表现之间的区分度。
通过逻辑回归方法，利用师生的音视频等课堂中网络情况监控数据，对老师认为的有网络问题的课堂进行技术上的分类，应用数据模型进一步判断其是否具有网络卡顿性质。该模型优点为分类结果一目了然且能得到属于网络问题课堂的概率值，可解释性强，运行速度快。
首先需要把收集上来的数据进行预处理，去掉一些不重要的影响因素，从而提高模型运行速度与准确性，然后代入模型得到该工单反映的课堂是否具有网络卡顿问题且能看到其属于网络卡顿课堂的概率大小，由此便可根据实际业务需要调整分类标准，从而减少放过问题课堂的可能性。


四、业务流程 
需要在教师提交网络状况工单时触发模型计算，返回结果触发工单自动关闭操作。

五、模型评估 
经过处理，最终使用的总样本量为正常课堂3868节，网络问题课堂646节，其中用于模型测试的课堂分别为775和128节。若把预测概率大于0.5识别为网络问题课堂，则模型的精确率和召回率分别为84%和68%，即预测为网络卡顿的课堂中，有84%的课堂确实有网络卡顿，同时在真的有网络卡顿问题的课堂中我们能预测正确的比例为68%。经计算，当前模型最佳阈值为0.21，若我们把预测概率大于0.2的课堂即标记为网络问题课堂，则模型的精确率和召回率分别为68%和80%。在牺牲了查准率的前提下，查全率有所提高，降低阈值可以少放过真正的问题课堂。

六、结论 
综上，该模型能在一定程度上提高网络工单处理率，进而降低网络工单率，实用性较强。若各业务方可以接受可当前结果，则可使用该模型进行灰测，期间不断优化调整，若效果持续变好，则一段时间后全量上线。
