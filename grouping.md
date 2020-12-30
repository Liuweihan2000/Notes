|           | Biden | corona | Trump | virus | president |
| :-------: | :---: | :----: | :---: | :---: | :-------: |
|   Biden   |   x   |   1    |  13   |   1   |     6     |
|  corona   |   x   |   x    |   3   |  12   |     0     |
|   Trump   |   x   |   x    |   x   |   3   |     7     |
|   virus   |   x   |   x    |   3   |   x   |     0     |
| president |   x   |   x    |   x   |   x   |     x     |

寻找连通图：

group one:

Biden -> Trump && President

Trump -> president

president -> null

end

grouping result: Biden Trump president



group two:

corona -> virus

virus -> null

end

grouping result: corona virus







Grouping算法

输入：

- 一个一维数组int[] index，用来表示单词的索引
- 一个二维数组int\[][] graph，用来寻找group

采用绝对聚类

输出：

- vector<vector<string>> result用来表示聚类后的组