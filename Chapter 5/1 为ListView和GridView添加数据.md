ListView���ô�ֱ�ѵ��÷�ʽ��ʾ���ݣ���GridView�����ˮƽ�ѵ��÷�ʽ��

����Ļ�����Ƕ���ࡣ

```
    <Grid Name="grid1" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <ListView x:Name="listView1" SelectionChanged="listView1_SelectionChanged">
            <x:String>Item 1</x:String>
            <x:String>Item 2</x:String>
        </ListView>

        <GridView x:Name="gridView1" SelectionChanged="gridView1_SelectionChanged">
            <x:String>Item 1</x:String>
            <x:String>Item 2</x:String>
        </GridView>
    </Grid>
```

��Ȼ��Ҳ�����ں�̨��������ӡ���ֻ��Ϊ�˽����Ƿ���һ��Ƚ϶��ѣ���Щ�����һ��϶��Ǻܳ�ġ�

```
ListView listView1 = new ListView();
listView1.Items.Add("Item 1");
listView1.Items.Add("Item 2");
listView1.Items.Add("Item 3");
listView1.SelectionChanged += listView1_SelectionChanged;           
grid1.Children.Add(listView1);        

GridView gridView1 = new GridView();
gridView1.Items.Add("Item 1");
gridView1.Items.Add("Item 2");
gridView1.SelectionChanged += gridView1_SelectionChanged;                                                                                                                        
grid1.Children.Add(gridView1);               
```

���ֻ��������������������ݻ᲻��Ƚ��鷳�أ�����Ҳ���԰���ЩItem 1��Item 2֮���ȫ������List�С�

```
List<String> itemsList = new List<string>();
itemsList.Add("Item 1");
itemsList.Add("Item 2");

ListView listView1 = new ListView();
listView1.ItemsSource = itemsList;
listView1.SelectionChanged += listView1_SelectionChanged;

grid1.Children.Add(listView1);
```
����һ������ʾ��ListView�������У��ǳ���ª����ȫ���ܹ�����Ҫ����ô���ǿ���������ItemTemplate���������������һЩ������������ʾ�����ǿ�����Grid��дһ��Image��ͷ����TextBlock���û���ID������һ��TextBlock���û�����Ϣ����������д�߿�ѽʲô�ġ�����Щ���߰����Binding֮��ģ��Ժ�����Ҳ��һ�𽲵�Ŷ������ֻҪ���������ݰ󶨾ͺá�
```
<Page.Resources>
    <CollectionViewSource x:Name="collectionVS" Source="{Binding Items}"/>
</Page.Resources>
   
<Grid Name="grid1" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <ListView x:Name="listView1"  ItemsSource="{Binding Source={StaticResource collectionVS}}"
      SelectionChanged="listView1_SelectionChanged">
        <ListView.ItemTemplate>
            <DataTemplate>
                <Grid>
                </Grid>
            </DataTemplate>
        </ListView.ItemTemplate>
    </ListView>      
</Grid>
```
����������������Ŷ��ͨ��WrapGrid��������ЩItem�İڷŷ�ʽ��

![����дͼƬ����](http://img.blog.csdn.net/20150406215113141)

```
<Grid Name="grid1" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
   <ListView VerticalAlignment="Bottom">
           
        <ListView.ItemsPanel>
            <ItemsPanelTemplate>
                <WrapGrid Orientation="Vertical" MaximumRowsOrColumns="2"/>
            </ItemsPanelTemplate>
        </ListView.ItemsPanel>

        <Rectangle Height="100" Width="100" Fill="Wheat" />
        <Rectangle Height="100" Width="100" Fill="White" />
        <Rectangle Height="100" Width="100" Fill="Gainsboro" />
        <Rectangle Height="100" Width="100" Fill="Violet" />
        <Rectangle Height="100" Width="100" Fill="DarkBlue" />
        <Rectangle Height="100" Width="100" Fill="RosyBrown" />
        <Rectangle Height="100" Width="100" Fill="SaddleBrown" />
        <Rectangle Height="100" Width="100" Fill="AliceBlue" />
        <Rectangle Height="100" Width="100" Fill="Fuchsia" />
        <Rectangle Height="100" Width="100" Fill="Aqua" />
        <Rectangle Height="100" Width="100" Fill="Tan" />
    </ListView>
</Grid>
```

��Ȼ��������ListView��GridView���ԣ�֪���û�ѡ������һ���Ǻ���Ҫ�ġ�SelectionMode���Ծ�����ListView��GridView��ѡ��ģʽ��������������ޡ���չ��

�������������ѡ��������selectedItems�������ǻ�����ͨ��IsItemClickEnabled������ListView��GridView�ĵ���¼����������Ҫע�⽫SelectionMode����ΪNone��

```
private void listView1_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
     selectedItems = (List<object>)e.AddedItems;   
}    
```