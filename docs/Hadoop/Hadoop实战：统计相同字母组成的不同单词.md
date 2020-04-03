# Hadoop实战：统计相同字母组成的不同单词

####  1.项目说明  

一本英文书籍包含成千上万个单词或者短语，现在我们需要在大量的单词中，找出相同字母组成的所有单词。由于这些单词相互之间没有依赖关系，为了加快数据处理的速度，可以借助Hadoop中MapReduce编程模型的特点，快速的编写出并行计算程序，从而实现大量单词的快速分析。所以项目要求通过编写MapReduce代码实现该功能。

####  2.项目示例数据  

![1585818081450](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585818081450.png)

####  3.项目开发思路及重点分析  

 基于以上需求，我们最终是需要找出相同字母组成的不同单词，而且MapReduce处理key-value对形式的数据，所以说在编写MapReduce之前必须先明确map和reduce输入输出的key-value对，从后往前推，reduce需要处理的是把输入的相同key的value聚合起来，value很好确定，就是单词本身，那么怎么会产生相同的key呢？其实也很简单。单词是由字母组成的，相同的字母的随机组合也能组成不同的单词，所以说把单词拆分成字符数组，然后进行排序就可以得到相同的key,这样正是我们所需要的。这也是本项目的难点所在。  

#### 4.项目程序

`AnagramMain.java`主函数代码

```
package com.hadoop.example4;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.conf.Configured;
import org.apache.hadoop.fs.FileSystem;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.util.Tool;
import org.apache.hadoop.util.ToolRunner;
public class AnagramMain extends Configured implements Tool{
    
    @SuppressWarnings( "deprecation")
    @Override
    public  int run(String[] args) throws Exception {
        Configuration conf = new Configuration();
        
        //删除已经存在的输出目录
        Path mypath = new Path(args[1]);
        FileSystem hdfs = mypath.getFileSystem(conf);
         if (hdfs.isDirectory(mypath)) {
            hdfs.delete(mypath, true);
        }
        Job job = new Job(conf, "testAnagram");
        job.setJarByClass(AnagramMain. class);	//设置主类
        
        job.setMapperClass(AnagramMapper. class);	//Mapper
        job.setMapOutputKeyClass(Text. class);
        job.setMapOutputValueClass(Text. class);
        job.setReducerClass(AnagramReducer. class);	//Reducer
        job.setOutputKeyClass(Text. class);
        job.setOutputValueClass(Text. class);
        FileInputFormat.addInputPath(job, new Path(args[0]));	//设置输入路径
        FileOutputFormat. setOutputPath(job, new Path(args[1]));	//设置输出路径
        return job.waitForCompletion(true) ? 0 : 1;//提交作业
        
    }

    public static void main(String[] args) throws Exception{
		//数据的输入路径和输出路径
        String[] args0 = { "hdfs://192.168.159.130:9000/anagram/anagram.txt" ,
        "hdfs://192.168.159.130:9000/anagram/output"};
        int ec = ToolRunner.run( new Configuration(), new AnagramMain(), args0);
        System. exit(ec);
    }
}
```

`AnagramMapper`Mapper函数代码

```
package com.hadoop.example4;


import java.io.IOException;
import java.util.Arrays;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Mapper;

public class AnagramMapper extends Mapper< Object, Text, Text, Text> {

    private Text sortedText = new Text();
    private Text orginalText = new Text();

    public void map(Object key, Text value, Context context) throws IOException, InterruptedException {

        String word = value.toString();
        char[] wordChars = word.toCharArray();//单词转化为字符数组
        Arrays.sort(wordChars);//对字符数组按字母排序
        String sortedWord = new String(wordChars);//字符数组转化为字符串
        sortedText.set(sortedWord);//设置输出key的值
        orginalText.set(word);//设置输出value的值
        context.write( sortedText, orginalText );//map输出
    }

}
```

`AnagramReducer`Reducer函数代码

```
package com.hadoop.example4;

import java.io.IOException;
import java.util.StringTokenizer;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Reducer;

public class AnagramReducer extends Reducer< Text, Text, Text, Text> {
   
    private Text outputKey = new Text();
    private Text outputValue = new Text();

   
    public void reduce(Text anagramKey, Iterable< Text> anagramValues,
            Context context) throws IOException, InterruptedException {
            String output = "";
            //对相同字母组成的单词，使用 ~ 符号进行拼接
            for(Text anagam:anagramValues){
            	 if(!output.equals("")){
            		 output = output + "~" ;
            	 }
            	 output = output + anagam.toString() ;
            }
            StringTokenizer outputTokenizer = new StringTokenizer(output,"~" );
            //输出anagrams（字谜）大于2的结果
            if(outputTokenizer.countTokens()>=2)
            {
                    output = output.replace( "~", ",");
                    outputKey.set(anagramKey.toString());//设置key的值
                    outputValue.set(output);//设置value的值
                    context.write( outputKey, outputValue);//reduce
            }
    }

}
```

#### 5.项目运行

1）保证hadoop集群正常启动

2）数据集上传到集群指定目录

![1585818579704](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585818579704.png)

3）编写代码

![1585818615968](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585818615968.png)

4）运行程序

* 结果如下

#### ![1585818643146](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585818643146.png)

5）查看结果

![1585818824029](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585818824029.png)

![1585818811989](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585818811989.png)