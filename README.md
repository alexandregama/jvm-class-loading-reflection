# jvm-class-loading-reflection
JVM Class Loading and Reflection

### Delegation Model

There is a hierarchy of classloaders

Class loader **may** delegate to its parent

Paret may load class

We have 3 different class loaders

- Application Class loader

- Extension Class loader

- Bootstrap Class loader 

We can test the class loader delegation using the following code:

```java
import java.net.URLClassLoader;

public class Delegation {

	public static void main(String[] args) {
		URLClassLoader classloader = (URLClassLoader) ClassLoader.getSystemClassLoader();
		do {
			System.out.println(classloader);
		} while ((classloader = (URLClassLoader) classloader.getParent()) != null);
		System.out.println("Boostrap Classloader");
	}
	
}
```

As you can notice, the last one will be the Bootstrap Classloader. 

We will get

```bash
sun.misc.Launcher$AppClassLoader@14dad5dc
sun.misc.Launcher$ExtClassLoader@15615099
Boostrap Classloader
```

We are able to print all urls that classloader tries to get the result. With the following code, we can print all urls used:

```java
public class Delegation {

	public static void main(String[] args) {
		URLClassLoader classloader = (URLClassLoader) ClassLoader.getSystemClassLoader();
		do {
			System.out.println(classloader);
			for (URL url: classloader.getURLs()) {
				System.out.printf("\t %s\n", url.getPath());
			}
		} while ((classloader = (URLClassLoader) classloader.getParent()) != null);
		System.out.println("Boostrap Classloader");
	}
	
}

```

As you can see, the result will be something as the following:

```bash
sun.misc.Launcher$AppClassLoader@14dad5dc
	 /Users/developer/java/workspace/procurando-ape/target/test-classes/
	 /Users/developer/java/workspace/procurando-ape/target/classes/
	 /Users/developer/.m2/repository/javax/annotation/javax.annotation-api/1.2/javax.annotation-api-1.2.jar
	 /Users/developer/.m2/repository/javax/interceptor/javax.interceptor-api/1.2/javax.interceptor-api-1.2.jar
	 /Users/developer/.m2/repository/javax/ejb/javax.ejb-api/3.2/javax.ejb-api-3.2.jar
	 /Users/developer/.m2/repository/javax/transaction/javax.transaction-api/1.2/javax.transaction-api-1.2.jar
	 /Users/developer/.m2/repository/javax/validation/validation-api/1.1.0.Final/validation-api-1.1.0.Final.jar
	 /Users/developer/.m2/repository/javax/inject/javax.inject/1/javax.inject-1.jar
	 /Users/developer/.m2/repository/javax/enterprise/cdi-api/1.1/cdi-api-1.1.jar
	 /Users/developer/.m2/repository/javax/servlet/javax.servlet-api/3.1.0/javax.servlet-api-3.1.0.jar
	 /Users/developer/.m2/repository/javax/el/el-api/2.2/el-api-2.2.jar
	 /Users/developer/.m2/repository/javax/servlet/jstl/1.2/jstl-1.2.jar
	 /Users/developer/.m2/repository/junit/junit/4.12/junit-4.12.jar
	 /Users/developer/.m2/repository/org/hamcrest/hamcrest-core/1.3/hamcrest-core-1.3.jar
	 /Users/developer/.m2/repository/org/mockito/mockito-all/1.9.5/mockito-all-1.9.5.jar
	 /Users/developer/.m2/repository/org/hamcrest/hamcrest-all/1.3/hamcrest-all-1.3.jar
	 /Users/developer/.m2/repository/br/com/caelum/vraptor/4.1.1/vraptor-4.1.1.jar
	 /Users/developer/.m2/repository/com/google/guava/guava/15.0/guava-15.0.jar
	 /Users/developer/.m2/repository/br/com/caelum/iogi/1.0.0/iogi-1.0.0.jar
	 /Users/developer/.m2/repository/com/thoughtworks/paranamer/paranamer/2.6/paranamer-2.6.jar
	 /Users/developer/.m2/repository/net/vidageek/mirror/1.6.1/mirror-1.6.1.jar
	 /Users/developer/.m2/repository/org/javassist/javassist/3.18.1-GA/javassist-3.18.1-GA.jar
	 /Users/developer/.m2/repository/org/slf4j/slf4j-api/1.6.0/slf4j-api-1.6.0.jar
	 /Users/developer/.m2/repository/com/thoughtworks/xstream/xstream/1.4.7/xstream-1.4.7.jar
	 /Users/developer/.m2/repository/xmlpull/xmlpull/1.1.3.1/xmlpull-1.1.3.1.jar
	 /Users/developer/.m2/repository/xpp3/xpp3_min/1.1.4c/xpp3_min-1.1.4c.jar
	 /Users/developer/.m2/repository/com/google/code/gson/gson/2.2.4/gson-2.2.4.jar
	 /Users/developer/.m2/repository/org/jboss/weld/servlet/weld-servlet-core/2.1.2.Final/weld-servlet-core-2.1.2.Final.jar
	 /Users/developer/.m2/repository/org/jboss/weld/weld-spi/2.1.Final/weld-spi-2.1.Final.jar
	 /Users/developer/.m2/repository/org/jboss/weld/weld-api/2.1.Final/weld-api-2.1.Final.jar
	 /Users/developer/.m2/repository/org/jboss/spec/javax/el/jboss-el-api_3.0_spec/1.0.0.Alpha1/jboss-el-api_3.0_spec-1.0.0.Alpha1.jar
	 /Users/developer/.m2/repository/org/jboss/logging/jboss-logging/3.1.3.GA/jboss-logging-3.1.3.GA.jar
	 /Users/developer/.m2/repository/org/jboss/weld/weld-core-impl/2.1.2.Final/weld-core-impl-2.1.2.Final.jar
	 /Users/developer/.m2/repository/org/jboss/classfilewriter/jboss-classfilewriter/1.0.4.Final/jboss-classfilewriter-1.0.4.Final.jar
	 /Users/developer/.m2/repository/org/jboss/spec/javax/annotation/jboss-annotations-api_1.2_spec/1.0.0.Alpha1/jboss-annotations-api_1.2_spec-1.0.0.Alpha1.jar
	 /Users/developer/.m2/repository/org/jboss/spec/javax/interceptor/jboss-interceptors-api_1.2_spec/1.0.0.Alpha3/jboss-interceptors-api_1.2_spec-1.0.0.Alpha3.jar
	 /Users/developer/.m2/repository/br/com/caelum/vraptor/vraptor-i18n/4.0.1/vraptor-i18n-4.0.1.jar
	 /Users/developer/.m2/repository/org/jboss/weld/servlet/weld-servlet/2.1.2.Final/weld-servlet-2.1.2.Final.jar
	 /Users/developer/.m2/repository/org/hibernate/hibernate-validator-cdi/5.0.2.Final/hibernate-validator-cdi-5.0.2.Final.jar
	 /Users/developer/.m2/repository/org/hibernate/hibernate-validator/5.0.2.Final/hibernate-validator-5.0.2.Final.jar
	 /Users/developer/.m2/repository/com/fasterxml/classmate/1.0.0/classmate-1.0.0.jar
	 /Users/developer/.m2/repository/org/hibernate/hibernate-entitymanager/4.3.0.Final/hibernate-entitymanager-4.3.0.Final.jar
	 /Users/developer/.m2/repository/org/jboss/logging/jboss-logging-annotations/1.2.0.Beta1/jboss-logging-annotations-1.2.0.Beta1.jar
	 /Users/developer/.m2/repository/org/hibernate/hibernate-core/4.3.0.Final/hibernate-core-4.3.0.Final.jar
	 /Users/developer/.m2/repository/org/jboss/spec/javax/transaction/jboss-transaction-api_1.2_spec/1.0.0.Final/jboss-transaction-api_1.2_spec-1.0.0.Final.jar
	 /Users/developer/.m2/repository/dom4j/dom4j/1.6.1/dom4j-1.6.1.jar
	 /Users/developer/.m2/repository/xml-apis/xml-apis/1.0.b2/xml-apis-1.0.b2.jar
	 /Users/developer/.m2/repository/org/hibernate/common/hibernate-commons-annotations/4.0.4.Final/hibernate-commons-annotations-4.0.4.Final.jar
	 /Users/developer/.m2/repository/org/hibernate/javax/persistence/hibernate-jpa-2.1-api/1.0.0.Final/hibernate-jpa-2.1-api-1.0.0.Final.jar
	 /Users/developer/.m2/repository/antlr/antlr/2.7.7/antlr-2.7.7.jar
	 /Users/developer/.m2/repository/org/jboss/jandex/1.1.0.Final/jandex-1.1.0.Final.jar
	 /Users/developer/.m2/repository/org/hsqldb/hsqldb/2.3.0/hsqldb-2.3.0.jar
	 /Users/developer/.m2/repository/mysql/mysql-connector-java/5.1.6/mysql-connector-java-5.1.6.jar
	 /Users/developer/.m2/repository/org/slf4j/slf4j-log4j12/1.7.5/slf4j-log4j12-1.7.5.jar
	 /Users/developer/.m2/repository/log4j/log4j/1.2.17/log4j-1.2.17.jar
	 /Users/developer/.m2/repository/com/amazonaws/aws-java-sdk-s3/1.11.5/aws-java-sdk-s3-1.11.5.jar
	 /Users/developer/.m2/repository/com/amazonaws/aws-java-sdk-kms/1.11.5/aws-java-sdk-kms-1.11.5.jar
	 /Users/developer/.m2/repository/com/amazonaws/aws-java-sdk-core/1.11.5/aws-java-sdk-core-1.11.5.jar
	 /Users/developer/.m2/repository/commons-logging/commons-logging/1.1.3/commons-logging-1.1.3.jar
	 /Users/developer/.m2/repository/org/apache/httpcomponents/httpclient/4.5.2/httpclient-4.5.2.jar
	 /Users/developer/.m2/repository/org/apache/httpcomponents/httpcore/4.4.4/httpcore-4.4.4.jar
	 /Users/developer/.m2/repository/commons-codec/commons-codec/1.9/commons-codec-1.9.jar
	 /Users/developer/.m2/repository/com/fasterxml/jackson/core/jackson-databind/2.6.6/jackson-databind-2.6.6.jar
	 /Users/developer/.m2/repository/com/fasterxml/jackson/core/jackson-annotations/2.6.0/jackson-annotations-2.6.0.jar
	 /Users/developer/.m2/repository/com/fasterxml/jackson/core/jackson-core/2.6.6/jackson-core-2.6.6.jar
	 /Users/developer/.m2/repository/com/fasterxml/jackson/dataformat/jackson-dataformat-cbor/2.6.6/jackson-dataformat-cbor-2.6.6.jar
	 /Users/developer/.m2/repository/joda-time/joda-time/2.7/joda-time-2.7.jar
	 /Users/developer/.m2/repository/commons-fileupload/commons-fileupload/1.3/commons-fileupload-1.3.jar
	 /Users/developer/.m2/repository/commons-io/commons-io/2.2/commons-io-2.2.jar
	 /Users/developer/.m2/repository/org/apache/commons/commons-lang3/3.4/commons-lang3-3.4.jar
sun.misc.Launcher$ExtClassLoader@182decdb
	 /Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre/lib/ext/cldrdata.jar
	 /Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre/lib/ext/dnsns.jar
	 /Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre/lib/ext/jaccess.jar
	 /Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre/lib/ext/jfxrt.jar
	 /Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre/lib/ext/localedata.jar
	 /Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre/lib/ext/nashorn.jar
	 /Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre/lib/ext/sunec.jar
	 /Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre/lib/ext/sunjce_provider.jar
	 /Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre/lib/ext/sunpkcs11.jar
	 /Library/Java/JavaVirtualMachines/jdk1.8.0_60.jdk/Contents/Home/jre/lib/ext/zipfs.jar
	 /System/Library/Java/Extensions/MRJToolkit.jar
Boostrap Classloader
```

