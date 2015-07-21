�����ù�Windows Phone����Windows 8/8.1/10�����Ѷ��������Ž�ͼ�϶���İ���������ͨ��SemanticZoom��ʵ�ֵģ������ݹ���ʱ�����ֿؼ��������á�����һ���Ŵ���ͼZoomedInView��һ����С��ͼZoomedOutView��ǰ����Ҫ������ʾ��ǰҳ�����ϸ��Ϣ�������������ڿ��ٵ�����

![����дͼƬ����](http://img.blog.csdn.net/20150408210217453)

��ô�Ҿ��Լ�������ʵ����������������XAML����Ӵ��µĽ��棬���񻭻�Ҫ�Ȼ�����һ����

```
<Grid Name="grid1" Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <SemanticZoom x:Name="semanticZoom" VerticalAlignment="Center" HorizontalAlignment="Center">   
            <SemanticZoom.ZoomedOutView>
            
            </SemanticZoom.ZoomedOutView>

            <SemanticZoom.ZoomedInView>
              
            </SemanticZoom.ZoomedInView>

        </SemanticZoom>
    </Grid>
```
Ȼ��ֱ�����������ͼ���������Ҫ����Ķ���������ĺ��ľ��ǣ�ZoomedOutView��ZoomedInView����ʹ�õ�ͬһ��CollectionViewSource������Ϊ�Լ������ݼ��ġ���������������ڡ�ΪListView��GridView���顱̸������

�����ȰѺ�̨����д�ã��Ҿ���һƪ����װģ����дһ����ɡ�

```
    public class Alarm
    {
        public string Title { get; set; }
        public DateTime AlarmClockTime { get; set; }
        public string Description { get; set; }         
    }
```
Ȼ����һ�����������һ������ݡ���һ������ݡ�

```
private Alarm[] AddAlarmData()
{
     return new Alarm[]
     {
          new Alarm {Title="Alarm 1",AlarmClockTime=globalTime.AddHours(17),Description="First Alarm for Study" },
          new Alarm {Title="Alarm 2",AlarmClockTime=globalTime.AddHours(2),Description="Second Alarm for Study" },
         new Alarm {Title="Alarm 3",AlarmClockTime=globalTime.AddHours(7),Description="Third Alarm for Study" },
         new Alarm {Title="Alarm 4",AlarmClockTime=globalTime.AddHours(4),Description="4th Alarm for Study" },
         new Alarm {Title="Alarm 5",AlarmClockTime=globalTime.AddHours(5),Description="First Alarm for Fun" },
         new Alarm {Title="Alarm 6",AlarmClockTime=globalTime.AddHours(1),Description="First Alarm for Fun" },
         new Alarm {Title="Alarm 7",AlarmClockTime=globalTime.AddHours(15),Description="Second Alarm for Fun" },
         new Alarm {Title="Alarm 8",AlarmClockTime=globalTime.AddHours(9),Description="Third Alarm for Fun" },
         new Alarm {Title="Alarm 9",AlarmClockTime=globalTime.AddHours(20),Description="4th Alarm for Fun" },
         new Alarm {Title="Alarm 10",AlarmClockTime=globalTime.AddHours(14),Description="Second Alarm for Sleep" },
         new Alarm {Title="Alarm 11",AlarmClockTime=globalTime.AddHours(9),Description="First Alarm for Sleep" }
     };
}
```

��Ϊ�������Ҫ�ѷŴ���ͼ�����С��ͼ���ǵ���С��ͼ������һЩABCD֮�����ĸô�����������õ���ʱ�䣬�ͷֳ��������ϵȺ�������ͨ������������һ���������㶨��������һ����ֵ�ԣ���time��Ϊ�����������ٽ���Щ����ɸѡ�������󶨵�����ӵ�CollectionViewSource�С�����gridView1��gridView2�Ǽ�����ӵ�XAML�У���������Ȳ��һ���ٲ��ϡ�
```
Func<int, string> SwitchTime = (time) =>
{
    if (time <= 10 && time >= 6)
         return "����";
    else if (time > 10 && time < 14)
         return "����";
    else if (time >= 14 && time <= 20)
         return "����";
    else
         return "����";      
};
var varTime = from t in AddAlarmData()
              orderby t.AlarmClockTime.Hour
              group t by SwitchTime(t.AlarmClockTime.Hour);
CollectionViewSource collectionVS = new CollectionViewSource();
collectionVS.IsSourceGrouped = true;
collectionVS.Source = varTime;
this.gridView1.ItemsSource = collectionVS.View.CollectionGroups;
this.gridView2.ItemsSource = collectionVS.View;
```

��������д����ͼ��Ҳ���ǷŴ���ͼ����

```
<GridView x:Name="gridView2" IsSwipeEnabled="True" HorizontalAlignment="Center" VerticalAlignment="Center" ScrollViewer.IsHorizontalScrollChainingEnabled="False" Width="1800" Height="1000">
     <GridView.ItemTemplate>
         <DataTemplate>
             <StackPanel Orientation="Horizontal" Margin="12" 	    		 HorizontalAlignment="Left" Background="White">
                  <TextBlock Text="{Binding Title}" TextWrapping="Wrap" Foreground="Red"  FontFamily="Harrington"
				  Width="150" Height="100" FontSize="26" FontWeight="Light"/>
                  <TextBlock Text="{Binding AlarmClockTime}"  Foreground="Red" TextWrapping="Wrap" Width="150"  Height="100"      FontFamily="Harrington" FontSize="26" FontWeight="Light"/>
                  <TextBlock Text="{Binding Description}"  Foreground="Red" TextWrapping="Wrap" Width="150"  Height="100"       FontFamily="Harrington" FontSize="26" FontWeight="Light"/>
              </StackPanel>
          </DataTemplate>
      </GridView.ItemTemplate>
      <GridView.ItemsPanel>
           <ItemsPanelTemplate>
                <ItemsWrapGrid MaximumRowsOrColumns="8"/>
           </ItemsPanelTemplate>
      </GridView.ItemsPanel>
      <GridView.GroupStyle>
           <GroupStyle>
                 <GroupStyle.HeaderTemplate>
                      <DataTemplate>
                           <TextBlock Text='{Binding Key}' Foreground="{StaticResource ApplicationForegroundThemeBrush}" Margin="12" FontSize="30" FontFamily="���Ĳ���" FontWeight="ExtraBold" />
                      </DataTemplate>
                 </GroupStyle.HeaderTemplate>
            </GroupStyle>
      </GridView.GroupStyle>
</GridView>

```
���Ŵ�Ҷ��ܿ��ö��������Ժ��һ��ڽ�ͼ�����һЩע�͵�Ŷ��Ȼ������С��ͼ��

```
<GridView Name="gridView1" Background="Wheat" ScrollViewer.IsHorizontalScrollChainingEnabled="False" HorizontalAlignment="Center" VerticalAlignment="Center" Width="600" Height="200">
    <GridView.ItemTemplate>
        <DataTemplate>
            <TextBlock Width="100" Height="100" Text="{Binding Group.Key}" FontFamily="�����п�" FontWeight="Normal" FontSize="24" />
         </DataTemplate>
    </GridView.ItemTemplate>
    <GridView.ItemsPanel>
         <ItemsPanelTemplate>
              <ItemsWrapGrid ItemWidth="100" ItemHeight="100" MaximumRowsOrColumns="2"/>
          </ItemsPanelTemplate>
     </GridView.ItemsPanel>
     <GridView.ItemContainerStyle>
          <Style TargetType="GridViewItem">
              <Setter Property="Margin" Value="12" />
              <Setter Property="Padding" Value="3" />
              <Setter Property="BorderThickness" Value="1" />
              <Setter Property="Background" Value="Green"/>
          </Style>
     </GridView.ItemContainerStyle>
</GridView>
```

��ô����͵�����Ϊֹ�ˣ���������Ȼ���ǽ�ͼ�ˡ�

![����дͼƬ����](http://img.blog.csdn.net/20150408213431045)

![����дͼƬ����](http://img.blog.csdn.net/20150408213506738)

������ͼƬ���������Ļ����Ա��浽�������ٿ�����

���˽�������ص���Ϣ�����Կ��ھ��µġ�ʹ�ø������塱��