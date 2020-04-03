#  Eclipse下编写MapReduce程序运行并导出jar包上传远程Linux执行运行

1.Eclipse配置本地hadoop环境和安装插件  hadoop-eclipse-plugin-2.9.2.jar  以及配置hadoop环境

详见：

[Eclipse配置本地Hadoop开发环境](https://blog.csdn.net/weixin_44322234/article/details/105248496)

[Eclipse 安装 Hadoop 插件](https://blog.csdn.net/weixin_44322234/article/details/105251931)

上述准备工作做好后，下面开始编写MapReduce程序并执行以及导出jar包。

详细步骤如下：

第一步：编写MapReduce程序并执行

1、打开Eclipse，选择菜单栏的File->New->Project选项，出现如下图的对话框，选择"Map/Reduce Project"，如下图所示：

![1585735825549](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585735825549.png)

 2、新建MapReduce工程：如上图，点击"Next"，进入"Map/Reduce Project"，给工程命名为：hadoop，然后点击"Finish"。如下图所示： 



![1585735872212](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585735872212.png)



 新建的工程如下图所示： 

![1585735899858](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585735899858.png)

 3、新建class文件，选中上图工程名下的src，右击选择New->class，命名为WordCount，点击"Finish"。 

![1585735959271](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585735959271.png)

 4、编写MapReduce程序：一个Map函数，一个Reduce函数，一个主函数 

```
package com.hadoop.test;

	import java.io.IOException;  
	import java.util.StringTokenizer;  
	  
	import org.apache.hadoop.conf.Configuration;  
	import org.apache.hadoop.fs.Path;  
	import org.apache.hadoop.io.IntWritable;  
	import org.apache.hadoop.io.LongWritable;  
	import org.apache.hadoop.io.Text;  
	import org.apache.hadoop.mapreduce.Job;  
	import org.apache.hadoop.mapreduce.Mapper;  
	import org.apache.hadoop.mapreduce.Reducer;  
	import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;  
	import org.apache.hadoop.mapreduce.lib.input.TextInputFormat;  
	import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;  
	import org.apache.hadoop.mapreduce.lib.output.TextOutputFormat;  
	  
	public class WordCount {  
	  
	    public static class WordCountMap extends  
	            Mapper<LongWritable, Text, Text, IntWritable> {  
	  
	        private final IntWritable one = new IntWritable(1);  
	        private Text word = new Text();  
	  
	        public void map(LongWritable key, Text value, Context context)  
	                throws IOException, InterruptedException {  
	            String line = value.toString();  
	            StringTokenizer token = new StringTokenizer(line);  
	            while (token.hasMoreTokens()) {  
	                word.set(token.nextToken());  
	                context.write(word, one);  
	            }  
	        }  
	    }  

	    public static class WordCountReduce extends  
	            Reducer<Text, IntWritable, Text, IntWritable> {  
	  
	        public void reduce(Text key, Iterable<IntWritable> values,  
	                Context context) throws IOException, InterruptedException {  
	            int sum = 0;  
	            for (IntWritable val : values) {  
	                sum += val.get();  
	            }  
	            context.write(key, new IntWritable(sum));  
	        }  
	    }  
	  
	    public static void main(String[] args) throws Exception {  
	        Configuration conf = new Configuration();  	        
			@SuppressWarnings("deprecation")
			Job job = new Job(conf);  
	        job.setJarByClass(WordCount.class);  
	        job.setJobName("wordcount");  
	  
	        job.setOutputKeyClass(Text.class);  
	        job.setOutputValueClass(IntWritable.class);  
	  
	        job.setMapperClass(WordCountMap.class);  
	        job.setReducerClass(WordCountReduce.class);  
	  
	        job.setInputFormatClass(TextInputFormat.class);  
	        job.setOutputFormatClass(TextOutputFormat.class);  
	  
	        FileInputFormat.addInputPath(job, new Path(args[0]));  
	        FileOutputFormat.setOutputPath(job, new Path(args[1]));  
	  
	        job.waitForCompletion(true);  
	    }  
	}  

```

 5、创建输入文件夹in：在DFS Locations目录下的/user/chenbo/下创建in文件夹，右击选择"Create new directory"，创建好后，右击选择"Refresh"刷新，就能看到in文件夹 

![1585736118537](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585736118537.png)

 6、导入输入文件file1.txt，file2.txt：右击选择"Upload files to DFS"，添加file1.txt和file2.txt，点击"OK"。 

![1585736192958](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585736192958.png)

 7、运行MapReduce程序前的配置：选择菜单Run As->Run Configurations

![1585736269710](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585736269710.png)

![1585736362894](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585736362894.png)

![1585736460075](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585736460075.png)

8.Run 即可

第二步：导出JAR包

1、 右键单击工程，点开“Export…”，在弹出的对话框中选择“java/JAR file”， 

![1585736586828](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585736586828.png)

![1585736605932](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585736605932.png)

![1585736677321](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585736677321.png)

2.导出的jar包上传到远程hadoop安装路径下

![1585736955261](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585736955261.png)

3.执行命令

```
bin/hadoop jar WordCount.jar  /in /out
```

![1585736925310](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585736925310.png)

4.结果如图

![1585736855204](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585736855204.png)

![1585736818558](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585736818558.png)

