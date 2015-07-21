ǰ������ʹ����MessageDialog����Ϊ����������������һ�����Ӹߴ��ϵ�Toast֪ͨ��

Toast֪ͨ�����϶�������XML���ṩ�ģ�һ��ʼ�һ������Ų�֪��XMLԭ������ô������������ھ���������Toast��ص�֪ʶ��

1��ʵ����ToastNotification�ࡣ
```
ToastNotification toast1 = new ToastNotification(xdoc);            
```
2��ʹ��ToastNotificationManager������Toast֪ͨ��������ӡ�չʾ���Ƴ�����ȡ֪ͨ�ȵȡ�
```
ToastNotificationManager.CreateToastNotifier().Show(toast1);
```
3���ڵ�һ���е�xdoc����һ��XML���ݣ����Ӻζ����أ�
```
XmlDocument xdoc = new XmlDocument();
```
4����һ������ʵ������һ��XML��������û������ѽ�����ݴ������أ�

```
xdoc.LoadXml(txtXML.Text);
```
5����δ���ͽ�һ��Text������XML�С���Text�����кܶ��ֻ�ȡ��ʽ������������Ȼ���ᵽ��

Toast��XMLģ������࣬���ǿ���ֱ������ȡ���ǡ���ö�ٺ�ǿ���var���ɡ�

```
var items = Enum.GetNames(typeof(ToastTemplateType));
```

��ô����ʽ�����ˣ���Ϊ�ظ�������̫�����Ҿʹ��������2��Style��Դ��

```
	<Page.Resources>
        <Style TargetType="TextBlock" x:Key="StyleHeaderTextBlock">
            <Setter Property="FontSize" Value="40"/>
            <Setter Property="FontFamily"  Value="��������"/>
            <Setter Property="Foreground" Value="HotPink"/>
            <Setter Property="Margin" Value="12"/>
        </Style>
        
        <Style TargetType="Button" x:Key="StyleToastButton">
            <Setter Property="Width" Value="180"/>
            <Setter Property="Height" Value="50"/>
            <Setter Property="Background" Value="Aqua"/>
            <Setter Property="FontSize" Value="21"/>
            <Setter Property="Margin" Value="12"/>
            <Setter Property="Content" Value="��ʾToast֪ͨ" />
        </Style>
    </Page.Resources>
```
�����ǵ�һ������������Toast֪ͨ��
```
	   <StackPanel Orientation="Vertical" Grid.Column="0" Margin="12">
            <TextBlock Text="����Toast֪ͨ" Style="{StaticResource StyleHeaderTextBlock}"/>
            <StackPanel Orientation="Horizontal" HorizontalAlignment="Left">
                <TextBlock FontSize="24" Foreground="Wheat" Text="��ѡ��һ��ģ�壺" VerticalAlignment="Center"/>
                <ComboBox Name="comboBoxToast" Foreground="Green"  Width="275" 
                          SelectionChanged="comboBoxToast_SelectionChanged"/>
            </StackPanel>
            <TextBox Foreground="Green" x:Name="txtXML" HorizontalAlignment="Left" Width="500" Height="400" Header="ģ��XML��" TextWrapping="Wrap" FontSize="24"/>
            <Button Name="btnShowToast1" Click="btnShowToast1_Click"  Style="{StaticResource StyleToastButton}"/>
        </StackPanel>
```
��̨����Ҳ�����׵ģ��������潲�ľͺ��ˡ�

```
		public MainPage()
        {
            this.InitializeComponent();           
            var items = Enum.GetNames(typeof(ToastTemplateType));
            this.comboBoxToast.ItemsSource = items;
        }

        private void comboBoxToast_SelectionChanged(object sender, SelectionChangedEventArgs e)
        {
            string tempt = ((ComboBox)sender).SelectedItem as string;
            if (!string.IsNullOrEmpty(tempt))
            {               
                ToastTemplateType template = (ToastTemplateType)Enum.Parse(typeof(ToastTemplateType), tempt);
                XmlDocument xdoc = ToastNotificationManager.GetTemplateContent(template);
                txtXML.Text = xdoc.GetXml();
            }
        }

        private void btnShowToast1_Click(object sender, RoutedEventArgs e)
        {
            if (txtXML.Text == "")           
                return;          
            XmlDocument xdoc = new XmlDocument();
            xdoc.LoadXml(txtXML.Text);          
            ToastNotification toast1 = new ToastNotification(xdoc);            ToastNotificationManager.CreateToastNotifier().Show(toast1);         
        }
```
ģ���������õġ���

![����дͼƬ����](http://img.blog.csdn.net/20150517204957508)

![����дͼƬ����](http://img.blog.csdn.net/20150517205006354)

��src������ͼƬ��·��Ҳ������Toast����ʾͼ��Ŷ���Ͻ����԰ɡ���

�������ǵڶ���������ǰ��ĺ������ơ���

```
	<StackPanel Orientation="Vertical" Grid.Column="1">
            <TextBlock Text="����Toast֪ͨ����ʾ��" Style="{StaticResource StyleHeaderTextBlock}"/>
            <TextBlock Margin="12" Text="������Toast��Ϣ���ݣ�" FontSize="24"/>
            <TextBox Margin="12" Height="50" x:Name="txtMesaage"/>
            <TextBlock  Margin="12" FontSize="24" Text="��ѡ��һ����ʾ������"/>
            <ComboBox Margin="12" Height="50" x:Name="comboBoxAudio" Width="400" HorizontalAlignment="Left">
                <ComboBoxItem IsSelected="True">ms-winsoundevent:Notification.Default</ComboBoxItem>
                <ComboBoxItem>ms-winsoundevent:Notification.IM</ComboBoxItem>
                <ComboBoxItem>ms-winsoundevent:Notification.Mail</ComboBoxItem>
                <ComboBoxItem>ms-winsoundevent:Notification.Reminder</ComboBoxItem>
                <ComboBoxItem>ms-winsoundevent:Notification.Looping.Alarm</ComboBoxItem>
                <ComboBoxItem>ms-winsoundevent:Notification.Looping.Call</ComboBoxItem>
            </ComboBox>
            <StackPanel Orientation="Horizontal">
                <CheckBox x:Name="checkBoxLoop" Margin="12" Content="ѭ������"/>
                <CheckBox x:Name="checkBoxSilent" Margin="12" Content="����"/>
            </StackPanel>           
            <Button Name="btnShowToast2" Click="btnShowToast2_Click"  Style="{StaticResource StyleToastButton}"/>
        </StackPanel>
```
��������еġ�ms-winsoundevent:Notification.Default�������src�е�����������������������loop��silent�������Ƿ�ѭ���Լ��Ƿ������ǵ��׸���ô���أ�Ӧ�ý���Щ����ȫ�������뵽XML�С�

```
xmlContent = string.Format(
     "<toast duration='{0}'>" +
           "<visual>" +
                "<binding template='ToastText01'>" +
                      "<text id='1'>{1}</text>" +
                "</binding>" +
           "</visual>" +
		   "<audio src='{2}' loop='{3}' silent='{4}'/>" +
     "</toast>",
     toastDuration, msg, audioSrc, isLoop, isSilent    
);             
```
�����õ�xmlContentҲҪ�ȶ��������һ��ʼ����ΪEmpty�ͺá�
```
string xmlContent = string.Empty;  
```
isLoop��isSilent���Զ����Խ�������Ŀ�������CheckBox�л�ȡ����

```
string isLoop = checkBoxLoop.IsChecked == true ? "true" : "false";                                
string audioSrc = (comboBoxAudio.SelectedItem as ComboBoxItem).Content.ToString();          
string toastDuration = checkBoxLoop.IsChecked == true ? "long" : "short";            
string isSilent = checkBoxSilent.IsChecked == true ? "true" : "false";        
```
��Ȼ�����ǵø����ܵ�Щ���û������ڻ�û������֪ͨ���ݾ͵�����ʾToast֪ͨ��ť���Դ�����Ŀ�����Ҳ�Ǽ��õ�ѡ��
```
string msg = txtMesaage.Text == "" ? "�㻹û������Toast֪ͨ�������ء���" : txtMesaage.Text;   
```
��Щ׼��������д�����Ժ��ؾ͸�����Toast֪ͨ�ˣ��������Toast1����Ŷ��������ԡ�

������Щ֪ͨ��û��ʱ���Կ��ԣ���Ϊ��ʱ��������Ҫ����һ��ʱ����ִ��Toast֪ͨ������ȻҲ�ǿ���ʵ�ֵġ�

�������½�����ơ�

```
	  <StackPanel Orientation="Vertical" Grid.Column="2">
            <TextBlock Text="�ƻ�ʱ����ʾToast֪ͨ" Style="{StaticResource StyleHeaderTextBlock}"/>  
            <StackPanel Orientation="Horizontal" Height="60">
                <TextBlock FontSize="28" Text="�ƻ���"  VerticalAlignment="Center"/>
                <TextBox Name="tBoxTime" FontSize="28" Width="60" Height="45" VerticalAlignment="Center"/>
                <TextBlock FontSize="28" Text="�����ʾToast֪ͨ" VerticalAlignment="Center"/>
            </StackPanel>                
            <Button Name="btnShowToast3" Click="btnShowToast3_Click"  Style="{StaticResource StyleToastButton}"/>
        </StackPanel>
```
��̨�������¡�

```
	    private async void btnShowToast3_Click(object sender, RoutedEventArgs e)
        {
            int toastTime;
            try
            {                      
                toastTime = int.Parse(tBoxTime.Text.ToString());
                XmlDocument xdoc = ToastNotificationManager.GetTemplateContent(ToastTemplateType.ToastText01);                
                var txtnodes = xdoc.GetElementsByTagName("text");                
                txtnodes[0].InnerText = "��ã�����һ����ʱΪ"+toastTime.ToString()+ "���Toast��Ϣ��";              
                ScheduledToastNotification toast3 = new ScheduledToastNotification(xdoc, DateTimeOffset.Now.AddSeconds(toastTime));                ToastNotificationManager.CreateToastNotifier().AddToSchedule(toast3);
            }
            catch (Exception ex)
            {                  
                Windows.UI.Popups.MessageDialog messageDialog =    
                    new Windows.UI.Popups.MessageDialog(ex.Message);
                await messageDialog.ShowAsync();
            }             
        }   
```
�����С��������Ϊ�����ڽ��ⶨʱ����Toast��֪ͨ��ʽ����˾�ѡ���˱Ƚϼ򵥵�ToastText01ģ�塣�����ҳ�Text�ڵ㣬����ýڵ�д�����ݡ������Ǵ���Toast֪ͨ�ˡ�
```
ScheduledToastNotification toast3 = new ScheduledToastNotification(xdoc, DateTimeOffset.Now.AddSeconds(toastTime));
```
ͬ��Ϊ�˷�ֹ�û�û����TextBox������ʱ��������˴����ʽ��ʱ����硰5�롱������try��catch�쳣��⡣��Ȼ�ˣ���ʵ�ʲ�Ʒ�У�����ɾ�Ҫ���ø��������ˣ�ʱ����TextBox�����벢����һ�����£���Ӧ������ǰ����ܵ�TimePicker��

����3�γ�������ȫ�Ҹ���~

![����дͼƬ����](http://img.blog.csdn.net/20150517210525929)