## kotlin v1.9.0-Beta / micronaut v3 bug reproducer

This codebase reproduces a build failure when upgrading a Micronaut v3 codebase to the new
release of Kotlin (`1.9.0-Beta`).

## Stacktrace

The immediate past commit breaks simply by upgrading to the new version. The tests show the
following exception, but in general DI seems broken, as we get JPA initialization errors in
our environment:

```
Caused by: org.junit.jupiter.api.extension.TestInstantiationException: @MicronautTest used on test but no bean definition for the test present. This error indicates a misconfigured build or IDE. Please add the 'micronaut-inject-java' annotation processor to your test processor path (for Java this is the testAnnotationProcessor scope, for Kotlin kaptTest and for Groovy testCompile). See the documentation for reference: https://micronaut-projects.github.io/micronaut-test/latest/guide/
```

<details>
<summary>Click for full stacktrace</summary>
<pre>
org.junit.jupiter.engine.execution.ConditionEvaluationException: Failed to evaluate condition [io.micronaut.test.extensions.junit5.MicronautJunit5Extension]: @MicronautTest used on test but no bean definition for the test present. This error indicates a misconfigured build or IDE. Please add the 'micronaut-inject-java' annotation processor to your test processor path (for Java this is the testAnnotationProcessor scope, for Kotlin kaptTest and for Groovy testCompile). See the documentation for reference: https://micronaut-projects.github.io/micronaut-test/latest/guide/
	at app//org.junit.jupiter.engine.execution.ConditionEvaluator.evaluationException(ConditionEvaluator.java:81)
	at app//org.junit.jupiter.engine.execution.ConditionEvaluator.evaluate(ConditionEvaluator.java:69)
	at app//org.junit.jupiter.engine.execution.ConditionEvaluator.lambda$evaluate$0(ConditionEvaluator.java:55)
	at java.base@20.0.1/java.util.stream.ReferencePipeline$3$1.accept(ReferencePipeline.java:197)
	at java.base@20.0.1/java.util.stream.ReferencePipeline$2$1.accept(ReferencePipeline.java:179)
	at java.base@20.0.1/java.util.stream.StreamSpliterators$WrappingSpliterator.tryAdvance(StreamSpliterators.java:300)
	at java.base@20.0.1/java.util.stream.Streams$ConcatSpliterator.tryAdvance(Streams.java:723)
	at java.base@20.0.1/java.util.stream.Streams$ConcatSpliterator.tryAdvance(Streams.java:720)
	at java.base@20.0.1/java.util.stream.ReferencePipeline.forEachWithCancel(ReferencePipeline.java:129)
	at java.base@20.0.1/java.util.stream.AbstractPipeline.copyIntoWithCancel(AbstractPipeline.java:527)
	at java.base@20.0.1/java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:513)
	at java.base@20.0.1/java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:499)
	at java.base@20.0.1/java.util.stream.FindOps$FindOp.evaluateSequential(FindOps.java:150)
	at java.base@20.0.1/java.util.stream.AbstractPipeline.evaluate(AbstractPipeline.java:234)
	at java.base@20.0.1/java.util.stream.ReferencePipeline.findFirst(ReferencePipeline.java:647)
	at app//org.junit.jupiter.engine.execution.ConditionEvaluator.evaluate(ConditionEvaluator.java:57)
	at app//org.junit.jupiter.engine.descriptor.JupiterTestDescriptor.shouldBeSkipped(JupiterTestDescriptor.java:202)
	at app//org.junit.jupiter.engine.descriptor.JupiterTestDescriptor.shouldBeSkipped(JupiterTestDescriptor.java:57)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$checkWhetherSkipped$3(NodeTestTask.java:131)
	at app//org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.checkWhetherSkipped(NodeTestTask.java:131)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.execute(NodeTestTask.java:92)
	at java.base@20.0.1/java.util.ArrayList.forEach(ArrayList.java:1511)
	at app//org.junit.platform.engine.support.hierarchical.SameThreadHierarchicalTestExecutorService.invokeAll(SameThreadHierarchicalTestExecutorService.java:41)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$6(NodeTestTask.java:155)
	at app//org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$8(NodeTestTask.java:141)
	at app//org.junit.platform.engine.support.hierarchical.Node.around(Node.java:137)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$9(NodeTestTask.java:139)
	at app//org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.executeRecursively(NodeTestTask.java:138)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.execute(NodeTestTask.java:95)
	at java.base@20.0.1/java.util.ArrayList.forEach(ArrayList.java:1511)
	at app//org.junit.platform.engine.support.hierarchical.SameThreadHierarchicalTestExecutorService.invokeAll(SameThreadHierarchicalTestExecutorService.java:41)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$6(NodeTestTask.java:155)
	at app//org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$8(NodeTestTask.java:141)
	at app//org.junit.platform.engine.support.hierarchical.Node.around(Node.java:137)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.lambda$executeRecursively$9(NodeTestTask.java:139)
	at app//org.junit.platform.engine.support.hierarchical.ThrowableCollector.execute(ThrowableCollector.java:73)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.executeRecursively(NodeTestTask.java:138)
	at app//org.junit.platform.engine.support.hierarchical.NodeTestTask.execute(NodeTestTask.java:95)
	at app//org.junit.platform.engine.support.hierarchical.SameThreadHierarchicalTestExecutorService.submit(SameThreadHierarchicalTestExecutorService.java:35)
	at app//org.junit.platform.engine.support.hierarchical.HierarchicalTestExecutor.execute(HierarchicalTestExecutor.java:57)
	at app//org.junit.platform.engine.support.hierarchical.HierarchicalTestEngine.execute(HierarchicalTestEngine.java:54)
	at app//org.junit.platform.launcher.core.EngineExecutionOrchestrator.execute(EngineExecutionOrchestrator.java:147)
	at app//org.junit.platform.launcher.core.EngineExecutionOrchestrator.execute(EngineExecutionOrchestrator.java:127)
	at app//org.junit.platform.launcher.core.EngineExecutionOrchestrator.execute(EngineExecutionOrchestrator.java:90)
	at app//org.junit.platform.launcher.core.EngineExecutionOrchestrator.lambda$execute$0(EngineExecutionOrchestrator.java:55)
	at app//org.junit.platform.launcher.core.EngineExecutionOrchestrator.withInterceptedStreams(EngineExecutionOrchestrator.java:102)
	at app//org.junit.platform.launcher.core.EngineExecutionOrchestrator.execute(EngineExecutionOrchestrator.java:54)
	at app//org.junit.platform.launcher.core.DefaultLauncher.execute(DefaultLauncher.java:114)
	at app//org.junit.platform.launcher.core.DefaultLauncher.execute(DefaultLauncher.java:86)
	at app//org.junit.platform.launcher.core.DefaultLauncherSession$DelegatingLauncher.execute(DefaultLauncherSession.java:86)
	at app//org.junit.platform.launcher.core.SessionPerRequestLauncher.execute(SessionPerRequestLauncher.java:53)
	at org.gradle.api.internal.tasks.testing.junitplatform.JUnitPlatformTestClassProcessor$CollectAllTestClassesExecutor.processAllTestClasses(JUnitPlatformTestClassProcessor.java:99)
	at org.gradle.api.internal.tasks.testing.junitplatform.JUnitPlatformTestClassProcessor$CollectAllTestClassesExecutor.access$000(JUnitPlatformTestClassProcessor.java:79)
	at org.gradle.api.internal.tasks.testing.junitplatform.JUnitPlatformTestClassProcessor.stop(JUnitPlatformTestClassProcessor.java:75)
	at org.gradle.api.internal.tasks.testing.SuiteTestClassProcessor.stop(SuiteTestClassProcessor.java:62)
	at java.base@20.0.1/jdk.internal.reflect.DirectMethodHandleAccessor.invoke(DirectMethodHandleAccessor.java:104)
	at java.base@20.0.1/java.lang.reflect.Method.invoke(Method.java:578)
	at org.gradle.internal.dispatch.ReflectionDispatch.dispatch(ReflectionDispatch.java:36)
	at org.gradle.internal.dispatch.ReflectionDispatch.dispatch(ReflectionDispatch.java:24)
	at org.gradle.internal.dispatch.ContextClassLoaderDispatch.dispatch(ContextClassLoaderDispatch.java:33)
	at org.gradle.internal.dispatch.ProxyDispatchAdapter$DispatchingInvocationHandler.invoke(ProxyDispatchAdapter.java:94)
	at jdk.proxy1/jdk.proxy1.$Proxy2.stop(Unknown Source)
	at org.gradle.api.internal.tasks.testing.worker.TestWorker$3.run(TestWorker.java:193)
	at org.gradle.api.internal.tasks.testing.worker.TestWorker.executeAndMaintainThreadName(TestWorker.java:129)
	at org.gradle.api.internal.tasks.testing.worker.TestWorker.execute(TestWorker.java:100)
	at org.gradle.api.internal.tasks.testing.worker.TestWorker.execute(TestWorker.java:60)
	at org.gradle.process.internal.worker.child.ActionExecutionWorker.execute(ActionExecutionWorker.java:56)
	at org.gradle.process.internal.worker.child.SystemApplicationClassLoaderWorker.call(SystemApplicationClassLoaderWorker.java:113)
	at org.gradle.process.internal.worker.child.SystemApplicationClassLoaderWorker.call(SystemApplicationClassLoaderWorker.java:65)
	at app//worker.org.gradle.process.internal.worker.GradleWorkerMain.run(GradleWorkerMain.java:69)
	at app//worker.org.gradle.process.internal.worker.GradleWorkerMain.main(GradleWorkerMain.java:74)
Caused by: org.junit.jupiter.api.extension.TestInstantiationException: @MicronautTest used on test but no bean definition for the test present. This error indicates a misconfigured build or IDE. Please add the 'micronaut-inject-java' annotation processor to your test processor path (for Java this is the testAnnotationProcessor scope, for Kotlin kaptTest and for Groovy testCompile). See the documentation for reference: https://micronaut-projects.github.io/micronaut-test/latest/guide/
	at app//io.micronaut.test.extensions.junit5.MicronautJunit5Extension.evaluateExecutionCondition(MicronautJunit5Extension.java:245)
	at app//org.junit.jupiter.engine.execution.ConditionEvaluator.evaluate(ConditionEvaluator.java:64)
	... 73 more
</pre>
</details>

## Micronaut 3.9.2 Documentation

- [User Guide](https://docs.micronaut.io/3.9.2/guide/index.html)
- [API Reference](https://docs.micronaut.io/3.9.2/api/index.html)
- [Configuration Reference](https://docs.micronaut.io/3.9.2/guide/configurationreference.html)
- [Micronaut Guides](https://guides.micronaut.io/index.html)
---

- [Shadow Gradle Plugin](https://plugins.gradle.org/plugin/com.github.johnrengelman.shadow)
- [Micronaut Gradle Plugin documentation](https://micronaut-projects.github.io/micronaut-gradle-plugin/latest/)
- [GraalVM Gradle Plugin documentation](https://graalvm.github.io/native-build-tools/latest/gradle-plugin.html)
## Feature http-client documentation

- [Micronaut HTTP Client documentation](https://docs.micronaut.io/latest/guide/index.html#httpClient)


## Feature github-workflow-ci documentation

- [https://docs.github.com/en/actions](https://docs.github.com/en/actions)


