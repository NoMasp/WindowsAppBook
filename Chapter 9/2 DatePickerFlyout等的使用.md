��һƪ������WP��DataPickerFlyout��TimePickerFlyout��ʹ�ã�������ֻ����WP��Ŷ��Ҳ����˲Ž���һ�ڷŵ����һ�µġ�

```
<Grid Background="Blue">  
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
                       
    <StackPanel Grid.Row="0" Margin="12" Orientation="Vertical">     
        <Button Content="Let's Show DatePicker" >
            <Button.Flyout>        
                <DatePickerFlyout x:Name="datePickerFlyout" Title="ѡ������" 
                     DatePicked="datePickerFlyout_DatePicked" Closed="datePickerFlyout_Closed" />
            </Button.Flyout>
        </Button>
                
        <DatePicker Header="Date"  Margin="4" />          
        <TextBlock Name="textBlockDate" FontSize="20" Margin="4" />            
    </StackPanel>

    <StackPanel Grid.Row="1" Margin="12" Orientation="Vertical">
        <Button Content="Let's Show TimePicker" >
            <Button.Flyout>
                <TimePickerFlyout x:Name="timePickerFlyout" Title="ѡ��ʱ��" 
                    TimePicked="timePickerFlyout_TimePicked" Closed="timePickerFlyout_Closed" />
            </Button.Flyout>
        </Button>

        <TimePicker Header="Time" Margin="4" />
        <TextBlock Name="textBlockTime" FontSize="20" Margin="4"/>             
    </StackPanel>                                      
</Grid>
```

��̨�¼����£�

```
public sealed partial class MainPage : Page
{
    public MainPage()
    {
        this.InitializeComponent();

        this.NavigationCacheMode = NavigationCacheMode.Required;
    }

    protected override void OnNavigatedTo(NavigationEventArgs e)
    {
        // ��������1
        datePickerFlyout.Date = DateTimeOffset.Now.AddDays(1);

        // ���ÿ�ѡ��������ݺ���С���
        datePickerFlyout.MinYear = DateTimeOffset.Now.AddYears(-100);   
        datePickerFlyout.MaxYear = DateTimeOffset.Now.AddYears(100);

        // �˴�Ҳ������ꡰ�����¡������ա��е�ĳһ������ʾ
        datePickerFlyout.YearVisible = true;
        datePickerFlyout.MonthVisible = true;       
        datePickerFlyout.DayVisible = true;

        // ѡ�������
        // ��Gregorian ����, Hebrew ϣ������, Hijri ����, Japanese �ձ���, Julian ����������, Korean ������, Taiwan ̨����, Thai ̩����, UmAlQura ����������
        datePickerFlyout.CalendarIdentifier = CalendarIdentifiers.Gregorian;                                                 
		
        // Time - TimePicker �ؼ���ǰ��ʾ��ʱ��
        timePickerFlyout.Time = new TimeSpan(18, 0, 0);

        // ����TimePickerFlyout�ķ���ѡ��������ֵ�����
        timePickerFlyout.MinuteIncrement=2; 

        //����Ϊ24Сʱ�ƣ�Ҳ����Ϊ12Сʱ��
        timePickerFlyout.ClockIdentifier = ClockIdentifiers.TwentyFourHour;                 
    }

    // ���û����DatePicker����ɰ�ť�󼤻���¼�
    private void datePickerFlyout_DatePicked(DatePickerFlyout sender, DatePickedEventArgs args)
    {                      
        textBlockDate.Text = args.NewDate.ToString("yyyy-MM-dd hh:mm:ss");
        textBlockDate.Text += Environment.NewLine;
    }

    // ���û����DatePicker��ȡ����ť���ֻ��ķ��ذ�ť�󼤻���¼����������ɰ�ť��Ҳ�����ø��¼�
    private void datePickerFlyout_Closed(object sender, object e)
    {
        textBlockDate.Text += "You just close the DatePickerFlyout.";
        textBlockDate.Text += Environment.NewLine;
    }

    // ���û����TimePicker����ɰ�ť�󼤻���¼�
    private void timePickerFlyout_TimePicked(TimePickerFlyout sender, TimePickedEventArgs args)
    {
        // e.OldTime - ԭʱ��
        // e.NewTime - ��ʱ��
        textBlockTime.Text = args.NewTime.ToString("c");
        textBlockTime.Text += Environment.NewLine;
    }

    // ���û����TimePicker��ȡ����ť���ֻ��ķ��ذ�ť�󼤻���¼����������ɰ�ť��Ҳ�����ø��¼�
    private void timePickerFlyout_Closed(object sender, object e)
    {
        textBlockTime.Text += "You just close the TimePickerFlyout.";
        textBlockTime.Text += Environment.NewLine;
    }
}
```

�򵥵Ľ���Flyout�����ִ�����ʽ��һ�־��������ͨ��Button��Flyout���ԡ���һ����ͨ��FlyoutBase.AttachedFlyout���Ը��κε�FrameworkElement�����������

����FrameworkElement�ĸ���֪ʶ�����Է����������ӡ�

https://msdn.microsoft.com/zh-cn/library/vstudio/system.windows.frameworkelement(v=vs.100).aspx

https://msdn.microsoft.com/en-us/library/system.windows.frameworkelement(v=vs.110).aspx

��Flyout����6�ֲ�ͬ�����ͣ�Flyout��DatePickerFlyout��ListPickerFlyout��MenuFlyout��TimePickerFlyout��

ʱ����Ⱦ�ֱ��Show�����ˡ�

XAML���룺

```
	<Page.Resources>
        <Style TargetType="Button">
            <Setter Property="Margin" Value="12"/>
            <Setter Property="FontSize" Value="20"/>
            <Setter Property="Foreground"  Value="White"/>
            <Setter Property="Background" Value="Green"/>
        </Style>
    </Page.Resources>

    <Grid Background="Blue">
        <StackPanel Orientation="Vertical">
            <!-- Flyout -->
            <Button Content="Let's Show Flyout">
                <Button.Flyout>
                    <Flyout>
                        <StackPanel >
                            <TextBox PlaceholderText="What do you want to say?"/>
                            <Button HorizontalAlignment="Right" Content="Yup"/>
                        </StackPanel>
                    </Flyout>
                </Button.Flyout>
            </Button>

            <!-- DatePickerFlyout -->
            <Button Content="Let's Show DatePicker" HorizontalAlignment="Right">
                <Button.Flyout>
                    <DatePickerFlyout Title="You need to choose Date: "  DatePicked="DatePickerFlyout_DatePicked"/>
                </Button.Flyout>
            </Button>

            <!-- ListPickerFlyout -->
            <Button Content="Let's Show ListPicker" >
                <Button.Flyout>
                    <ListPickerFlyout x:Name="listPickerFlyout" Title="ѡ�����ϵͳ��" ItemsPicked="listPickerFlyout_ItemsPicked"  >
                        <ListPickerFlyout.ItemTemplate>
                            <DataTemplate>
                                <TextBlock Text="{Binding}" FontSize="30"></TextBlock>
                            </DataTemplate>
                        </ListPickerFlyout.ItemTemplate>
                    </ListPickerFlyout>
                </Button.Flyout>
            </Button>

            <!--  MenuFlyout -->
            <Button x:Name="menuFlyoutButton" Content="Let's Show MenuFlyout" HorizontalAlignment="Right">
                <Button.Flyout >
                    <MenuFlyout>
                        <MenuFlyoutItem Text="You just say yes?" Click="MenuFlyoutItem_Click"/>
                        <MenuFlyoutItem Text="You just say no?" Click="MenuFlyoutItem_Click"/>
                        <MenuFlyoutItem Text="You say nothing..." Click="MenuFlyoutItem_Click"/>
                    </MenuFlyout>
                </Button.Flyout>
            </Button>

            <!--  PickerFlyout -->
            <Button Content="Let's Show Picker" >
                <Button.Flyout>
                    <PickerFlyout  Confirmed="PickerFlyout_Confirmed" ConfirmationButtonsVisible="True">
                        <TextBlock Text="Are you ok?" FontSize="30" Margin="0 100 0 0"/>
                    </PickerFlyout>
                </Button.Flyout>
            </Button>

            <!-- TimePickerFlyout -->
            <Button Content="Let's Show TimePicker" HorizontalAlignment="Right">
                <Button.Flyout>
                    <TimePickerFlyout Title="You need to choose Time: "  TimePicked="TimePickerFlyout_TimePicked"/>
                </Button.Flyout>
            </Button>

            <!-- FlyoutBase -->
            <TextBlock Text="Game Over" Margin="12" Foreground="Wheat" Tapped="TextBlock_Tapped" FontSize="20">
                <FlyoutBase.AttachedFlyout>
                    <Flyout>
                        <TextBox Text="��Ӵ������Ŷ��"/>
                    </Flyout>
                </FlyoutBase.AttachedFlyout>
            </TextBlock>
        </StackPanel>
    </Grid>
```

��̨C#���룺

```
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            // ��List���ݵ�ListPickerFlyout
            listPickerFlyout.ItemsSource = new List<string> { "Windows 10", "Windows 8/8.1", "Windows 7", "Windows Vista", "Windows XP","Others" };
        }

        // DatePickerFlyout������ѡ���¼����˴��¼������ǰ������ڵ�MessageDialog�ؼ�
        private async void DatePickerFlyout_DatePicked(DatePickerFlyout sender, DatePickedEventArgs args)
        {
            await new MessageDialog(args.NewDate.ToString()).ShowAsync();
        }

        // ListPickerFlyout��ѡ���¼���ѡ���б��е�һ�����Ե����ķ�ʽ��ʾ����
        private async void listPickerFlyout_ItemsPicked(ListPickerFlyout sender, ItemsPickedEventArgs args)
        {
            if (sender.SelectedItem != null)
            {
                await new MessageDialog("You choose: " + sender.SelectedItem.ToString()).ShowAsync();
            }
        }

        // MenuFlyout�Ĳ˵�ѡ��ĵ���¼�����ѡ��ı��ĸ�ֵ��Content
        private void MenuFlyoutItem_Click(object sender, RoutedEventArgs e)
        {
            menuFlyoutButton.Content = (sender as MenuFlyoutItem).Text;
        }

        // PickerFlyout��ȷ���¼����˴��¼������ǰ����ַ�����MessageDialog�ؼ�
        private async void PickerFlyout_Confirmed(PickerFlyout sender, PickerConfirmedEventArgs args)
        {
            await new MessageDialog("You choose ok").ShowAsync();
        }

        // TimePickerFlyout��ʱ��ѡ���¼����˴��¼������ǰ�����ѡʱ���MessageDialog�ؼ�
        private async void TimePickerFlyout_TimePicked(TimePickerFlyout sender, TimePickedEventArgs args)
        {
            await new MessageDialog(args.NewTime.ToString()).ShowAsync();
        }          

        // ͨ��FlyoutBase.ShowAttachedFlyout������չʾ��Flyout�ؼ�
        private void TextBlock_Tapped(object sender, TappedRoutedEventArgs e)
        {
            FrameworkElement element = sender as FrameworkElement;
            if (element != null)
            {
                FlyoutBase.ShowAttachedFlyout(element);
            }
        }    
    }
```

���˴���͵������ˣ������Ž�ͼ��

![����дͼƬ����](http://img.blog.csdn.net/20150602225329700)

![����дͼƬ����](http://img.blog.csdn.net/20150602225401680)

![����дͼƬ����](http://img.blog.csdn.net/20150602225407574)

![����дͼƬ����](http://img.blog.csdn.net/20150602225412633)

![����дͼƬ����](http://img.blog.csdn.net/20150602225418848)