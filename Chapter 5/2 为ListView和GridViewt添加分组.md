���ĳнӡ�ΪListView��GridView������ݡ���

����һ���������Ѿ��˽������������ݰ󶨵�ListView��GridView������ȻҪ�õ��������ؼ���������Ϊ���ݷ��࣬��ô�����Ͳ��ɱ����Ҫ�����ܹ����顣�������󶨵�����Դ���������б����е�ÿ���������������Լ������ô��������ˡ�

һʱ����Ҳ�벻��ʲô��ΰ�����ӣ�����һ���򵥵����ӵ�ʱ����ListView��GridView�ɡ���ô������Ŀ�����һ���࣬�����Shared�¡����ݶ��Ǻܼ��׵ģ����ӵı��⡢ʱ�䡢��ע�ȣ�Ϊ������һ��Ŀ¼�ͼ���һ��AlarmMode��������ѧϰ������ɣ�ѧϰ���������󡭡�
```
public class Alarm
{
    public string Title { get; set; }
    public DateTime AlarmClockTime { get; set; }
    public string Description { get; set; }
    public string AlarmMode { get; set; }
}
```
```
public class AlarmMode
{
   public AlarmMode()
   {
       alarmMode = new ObservableCollection<Alarm>();
   }
   public string Name { get; set; }
   public ObservableCollection<Alarm> alarmMode { get; private set; }
}
```
���ȣ���������һ��ȫ�ֵ�ʱ�䣬Ȼ����ҳ�����ʱ��������������������һ�����壩��
```
DateTime globalTime;
```

```
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    DateTime.TryParse("1/1/2115", out globalTime);
    AddAlarm();
    AddAlarmMode();
}
```

һ���������ڿ�����

```
private void AddAlarm()
{
    List<Alarm> listAlarm = new List<Alarm>();
    listAlarm.Add(new Alarm()
    {
        Title = "Alarm1",
        Description = "First Alarm",
        AlarmClockTime = globalTime.AddHours(1),
        AlarmMode = "Alarm In Life"
    });
    listAlarm.Add(new Alarm()
    {
        Title = "Alarm2",
        Description = "Second Alarm",
        AlarmClockTime = globalTime.AddHours(11),
        AlarmMode = "Alarm In Life"
    });
    listAlarm.Add(new Alarm()
    {
        Title = "Alarm3",
        Description = "Third Alarm",
        AlarmClockTime = globalTime.AddDays(1),
        AlarmMode = "Alarm In Life"
    });

    listAlarm.Add(new Alarm()
    {
        Title = "Alarm1",
        Description = "First Alarm",
        AlarmClockTime = globalTime.AddHours(12),
        AlarmMode = "Alarm In Study"
    });
    listAlarm.Add(new Alarm()
    {
        Title = "Alarm2",
        Description = "Second Alarm",
        AlarmClockTime = globalTime.AddHours(15),
        AlarmMode = "Alarm In Study"
    });
    listAlarm.Add(new Alarm()
    {
        Title = "Alarm3",
        Description = "Third Alarm",
        AlarmClockTime = globalTime.AddMonths(1),
        AlarmMode = "Alarm In Study"
    });

    ar alarmSetting = from ala in listAlarm
                       group ala
                       by ala.AlarmMode
                       into alaSetting
                       orderby alaSetting.Key
                       select alaSetting;
    collectionVSAlarm.Source = alarmSetting;
}

private void AddAlarmMode()
{
    List<AlarmMode> listAlarmMode = new List<AlarmMode>();

    AlarmMode am1 = new AlarmMode();
    am1.Name = "Alarm In Life";
    am1.alarmMode.Add(new Alarm()
    {
        Title = "Alarm1",
        Description = "First Alarm",
        AlarmClockTime = globalTime.AddHours(1),
    });
    am1.alarmMode.Add(new Alarm()
    {
        Title = "Alarm2",
        Description = "Second Alarm",
        AlarmClockTime = globalTime.AddHours(11),
    });
    am1.alarmMode.Add(new Alarm()
    {
        Title = "Alarm3",
        Description = "Third Alarm",
        AlarmClockTime = globalTime.AddDays(1),
    });
    listAlarmMode.Add(am1);

 	AlarmMode am2 = new AlarmMode();
    am2.Name = "Alarm In Study";
    am2.alarmMode.Add(new Alarm()
    {
        Title = "Alarm1",
        Description = "First Alarm",
        AlarmClockTime = globalTime.AddHours(12),
    });
    am2.alarmMode.Add(new Alarm()
    {
        Title = "Alarm2",
        Description = "Second Alarm",
        AlarmClockTime = globalTime.AddHours(15),
    });
    am2.alarmMode.Add(new Alarm()
    {
        Title = "Alarm3",
        Description = "Third Alarm",
        AlarmClockTime = globalTime.AddMonths(1),
    });
    listAlarmMode.Add(am2);

	collectionVSAlarmMode.Source = listAlarmMode;
}
```

��Щ���ݶ������߰���������Ҵպ��ſ����������������������Ҷ�����List<>������ģ�������ͨ��Add������ӵ�listAlarm��listAlarmMode�м��ɡ�����ٴ�listAlarm�и���AlarmMode�������ݵ�alaSetting��ͬʱ��Ҫ����Keyֵ�����������ѡ�������ӵ�collectionVSAlarm��Source�����С��������Ҫ��MainPage.xaml�ж����Ŷ������<Page.Resource/>һ��������ҪItemsPath��������������Ӷ���Ӵ��

```
    <UserControl.Resources>
        <CollectionViewSource x:Name="collectionVSAlarm"     IsSourceGrouped="True"/>
        <CollectionViewSource x:Name="collectionVSAlarmMode"     IsSourceGrouped="True" ItemsPath="alarmMode"/>
    </UserControl.Resources>
```

Ȼ�����ǻ���Ҫ����һ��ListGridGroupStyle�����̳�GroupStyleSelector����������SelectGroupStyleCore���������ҷ���ListGridGroupStyleResource��Դ�������Դ�ڲ��ͺ������ж��壬�䶨����App.xaml�С���Ӧ�Ĵ������¿���

```
    public class ListGridGroupStyle : GroupStyleSelector
    {
        protected override GroupStyle SelectGroupStyleCore(object group, uint level)
        {
            return (GroupStyle)App.Current.Resources["ListGridGroupStyleResource"];
        }
    }
```

�������غ�֮�����Ҫ��ǰ���UserControl.Resources�м�������������������

```
    <local:ListGridGroupStyle x:Key="ListGridGroupStyleResource"/>
```

Ȼ��������һϵ�еĻ�����ʽ��App.xaml�оͺ�����������Դ�ļ���ʹ�������ں����ϵͳ����ѧϰ�������DataTemplate��GroupStyle������Դ�ֵ��У�ǰ����Templateģ�壬������Style������ݵ��Ű��Ҷ����������ǵ����ú�Keyֵ��

```
    <Application.Resources>
        <ResourceDictionary>
            <DataTemplate x:Key="dataTemplateListView">
                <StackPanel Width="700" Margin="10">
                    <StackPanel Orientation="Horizontal">
                        <TextBlock Text="{Binding Title}" FontWeight="Bold" Margin="12"/>
                        <TextBlock Text="{Binding AlarmClockTime}" TextWrapping="NoWrap" Margin="12"/>
                        <TextBlock Text="{Binding Description}" TextWrapping="NoWrap" Margin="12"/>
                    </StackPanel>
              
                </StackPanel>
            </DataTemplate>

            <GroupStyle x:Key="ListGridGroupStyleResource">
                <GroupStyle.HeaderTemplate>
                    <DataTemplate>
                        <Grid Background="LightGray"  >
                            <TextBlock Text='{Binding Key}' Foreground="CornflowerBlue" Margin="12"  />
                        </Grid>
                    </DataTemplate>
                </GroupStyle.HeaderTemplate>
            </GroupStyle>
        </ResourceDictionary>
    </Application.Resources>
```

��ô��Щ������Դ���������֮�����MainPage.xaml��������Щ�ý�ȥ��������Դ�ĵ���������������Ҫע�⣬��ʵ������΢����һ����ĳ�����ԣ����ƾ��Ѿ�������˱����ˡ�����ӵ��һ�����õ�����ϰ�ߺ���Ҫ��

```
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="auto"/>
            <ColumnDefinition Width="auto"/>
        </Grid.ColumnDefinitions>

        <GridView Grid.Column="0" ItemsSource="{Binding Source={StaticResource collectionVSAlarmMode}}"        
                  Margin="12,120,12,12" MaxHeight="600" >
            <GridView.ItemTemplate>
                <DataTemplate>
                    <StackPanel Margin="18">
                        <TextBlock Text="{Binding Title}" FontWeight="ExtraBold"   />
                        <TextBlock Text="{Binding AlarmClockTime}"  FontWeight="Light" TextWrapping="NoWrap"  />
                        <TextBlock Text="{Binding Description}" TextWrapping="NoWrap"  />
                    </StackPanel>
                </DataTemplate>
            </GridView.ItemTemplate>
            <GridView.ItemsPanel>
                <ItemsPanelTemplate>
                    <ItemsWrapGrid MaximumRowsOrColumns="2"/>
                </ItemsPanelTemplate>
            </GridView.ItemsPanel>

            <GridView.GroupStyle>
                <GroupStyle>
                    <GroupStyle.HeaderTemplate>
                        <DataTemplate>
                            <Grid Background="Green" Margin="12">
                                <TextBlock Text='{Binding Name}' 
                                           Foreground="Bisque" Margin="36"/>
                            </Grid>
                        </DataTemplate>
                    </GroupStyle.HeaderTemplate>    
                </GroupStyle>
            </GridView.GroupStyle>
        </GridView>

        <ListView Grid.Column="1" ItemsSource="{Binding Source={StaticResource collectionVSAlarm}}"     
                  ItemTemplate="{StaticResource dataTemplateListView}"    
                  GroupStyleSelector="{StaticResource ListGridGroupStyleResource}"          
                  Margin="120" />       
    </Grid>
```

![����дͼƬ����](http://img.blog.csdn.net/20150407220856910)

����д������̫���˰�������Ʒ��ʱ��ɵúúõ����ˡ�
