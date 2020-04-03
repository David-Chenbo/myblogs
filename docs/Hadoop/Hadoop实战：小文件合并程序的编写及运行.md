# Hadoop实战：小文件合并程序的编写及运行

数据集文件

链接：https://pan.baidu.com/s/15aSG7ITEIoxN7l-R8-bSgA 
提取码：u8b4

#### 1.项目背景  

在实际项目中，输入数据往往是由许多小文件组成，这里的小文件是指小于HDFS系统Block大小的文件（默认128M）， 然而每一个存储在HDFS中的文件、目录和块都映射为一个对象，存储在NameNode服务器内存中，通常占用150个字节。 如果有1千万个文件，就需要消耗大约3GB的内存空间。如果是10亿个文件呢，简直不可想象。所以目前很多公司采用的方法就是在数据进入 Hadoop 的 HDFS 系统之前对大量的小文件进行合并，从而节约对NameNode内存空间的占用。  

#### 2.项目数据准备（数据集）  

![1585815722486](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585815722486.png)

#### 3.项目思路分析  

基于项目的需求，我们通过下面几个步骤完成：

1．首先通过 globStatus()方法过滤掉 svn 格式的文件，获取 E:\73\73 目录下的其他所有文件路径。

2．然后循环第一步的所有文件路径，通过globStatus()方法获取所有 txt 格式的文件路径。

3．最后通过IOUtils.copyBytes(in, out, 4096, false)方法将数据集合并为7个大文件，并上传至 HDFS。

#### 4.项目程序  

```
package com.hadoop.example;

import java.io.IOException;
import java.net.URI;
import java.net.URISyntaxException;
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.FSDataInputStream;
import org.apache.hadoop.fs.FSDataOutputStream;
import org.apache.hadoop.fs.FileStatus;
import org.apache.hadoop.fs.FileSystem;
import org.apache.hadoop.fs.FileUtil;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.fs.PathFilter;
import org.apache.hadoop.io.IOUtils;
/**
 * function 合并小文件至 HDFS 
 * @author chenbo
 *
 */
public class MergeSmallFilesToHDFS {
	private static FileSystem fs = null;
	private static FileSystem local = null;
	/**
	 * @function main 
	 * @param args
	 * @throws IOException
	 * @throws URISyntaxException
	 */
	public static void main(String[] args) throws IOException,
			URISyntaxException {
		list();
	}

	/**
	 * 
	 * @throws IOException
	 * @throws URISyntaxException
	 */
	public static void list() throws IOException, URISyntaxException {
		// 读取hadoop文件系统的配置
		Configuration conf = new Configuration();
		//文件系统访问接口
		URI uri = new URI("hdfs://djt002:9000");
		//创建FileSystem对象
		fs = FileSystem.get(uri, conf);
		// 获得本地文件系统
		local = FileSystem.getLocal(conf);
		//过滤目录下的 svn 文件
		FileStatus[] dirstatus = local.globStatus(new Path("D://data/tvdata/*"),new RegexExcludePathFilter("^.*svn$"));
		//获取73目录下的所有文件路径
		Path[] dirs = FileUtil.stat2Paths(dirstatus);
		FSDataOutputStream out = null;
		FSDataInputStream in = null;
		for (Path dir : dirs) {
			//2012-09-17
			String fileName = dir.getName().replace("-", "");//文件名称
			//只接受日期目录下的.txt文件
			FileStatus[] localStatus = local.globStatus(new Path(dir+"/*"),new RegexAcceptPathFilter("^.*txt$"));
			// 获得日期目录下的所有文件
			Path[] listedPaths = FileUtil.stat2Paths(localStatus);
			//输出路径
			Path block = new Path("hdfs://djt002:9000/tv/"+ fileName + ".txt");
			System.out.println("合并后的文件名称："+fileName+".txt");
			// 打开输出流
			out = fs.create(block);			
			for (Path p : listedPaths) {
				in = local.open(p);// 打开输入流
				IOUtils.copyBytes(in, out, 4096, false); // 复制数据
				// 关闭输入流
				in.close();
			}
			if (out != null) {
				// 关闭输出流
				out.close();
			}
		}
		
	}

	/**
	 * 
	 * @function 过滤 regex 格式的文件
	 *
	 */
	public static class RegexExcludePathFilter implements PathFilter {
		private final String regex;

		public RegexExcludePathFilter(String regex) {
			this.regex = regex;
		}

		@Override
		public boolean accept(Path path) {
			// TODO Auto-generated method stub
			boolean flag = path.toString().matches(regex);
			return !flag;
		}

	}

	/**
	 * 
	 * @function 接受 regex 格式的文件
	 *
	 */
	public static class RegexAcceptPathFilter implements PathFilter {
		private final String regex;

		public RegexAcceptPathFilter(String regex) {
			this.regex = regex;
		}

		@Override
		public boolean accept(Path path) {
			// TODO Auto-generated method stub
			boolean flag = path.toString().matches(regex);
			return flag;
		}

	}
}

```

#### 5.项目运行

1）保证hadoop集群正常启动

![1585816126029](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585816126029.png)

2）保证数据集下载到本地
![1585815722486](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585815722486.png)

3)在新建的包名下编写实现小文件合并功能的类MergeSmallFilesToHDFS

![1585816237490](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585816237490.png)

4）路径为自己集群和本地数据的目录

![1585816306500](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585816306500.png)

5）运行并查看结果

![1585816325183](C:\Users\David\AppData\Roaming\Typora\typora-user-images\1585816325183.png)