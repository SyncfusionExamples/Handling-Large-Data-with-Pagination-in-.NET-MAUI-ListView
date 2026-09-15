# Handling large data with Pagination in .NET MAUI ListView (SfListView)

Handling Large Data with Pagination in .NET MAUI ListView.

## Sample

```xaml

<ContentPage.Behaviors>
    <local:SfListViewPagingBehavior />
</ContentPage.Behaviors>

<DataPager:SfDataPager
            x:Name="dataPager"
            Grid.Row="1"
            Margin="7.5,11.5,7.5,11.5"
            ButtonFontSize="14"
            ButtonSize="36"
            ButtonSpacing="8"
            HeightRequest="36"
            HorizontalOptions="Fill"
            NumericButtonCount="8"
            PageCount="8"
            PageSize="12"
            UseOnDemandPaging="True"
            VerticalOptions="Center" />

<ListView:SfListView
    x:Name="verticalListView"
    Grid.Row="0"
    AutoFitMode="DynamicHeight"
    SelectionMode="Single">
    <ListView:SfListView.ItemTemplate>
        <DataTemplate x:DataType="local:PlaceInfo">
            <Grid Margin="16,12,16,12" ColumnSpacing="12">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="44" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Border
                    Padding="0"
                    HeightRequest="44"
                    HorizontalOptions="Start"
                    WidthRequest="44">
                    <Border.StrokeShape>
                        <RoundRectangle CornerRadius="3" />
                    </Border.StrokeShape>
                    <Image
                        Grid.Column="0"
                        Aspect="Fill"
                        HeightRequest="44"
                        Source="{Binding Image}"
                        WidthRequest="44" />
                </Border>
                <Grid
                    Grid.Column="1"
                    RowSpacing="4"
                    VerticalOptions="Center">
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="*" />
                    </Grid.RowDefinitions>
                    <Label
                        Grid.Row="0"
                        CharacterSpacing="0.25"
                        FontFamily="Roboto-Regular"
                        FontSize="14"
                        Text="{Binding Name}"
                        TextColor="Black" />
                    <Label
                        Grid.Row="1"
                        CharacterSpacing="0.15"
                        FontFamily="Roboto-Regular"
                        FontSize="14"
                        LineBreakMode="TailTruncation"
                        Text="{Binding Description}"
                        TextColor="DimGray" />
                </Grid>
            </Grid>
        </DataTemplate>
    </ListView:SfListView.ItemTemplate>
</ListView:SfListView>
```

```c#
public class SfListViewPagingBehavior : Behavior<ContentPage>
{
    #region Fields
    private Syncfusion.Maui.ListView.SfListView? listView;
    private PagingViewModel? pagingViewModel;
    private SfDataPager? dataPager;

    #endregion

    protected override void OnAttachedTo(ContentPage bindable)
    {
        listView = bindable.FindByName<Syncfusion.Maui.ListView.SfListView>("verticalListView");
        dataPager = bindable.FindByName<SfDataPager>("dataPager");
        pagingViewModel = new PagingViewModel();
        listView.BindingContext = pagingViewModel;
        dataPager.PageCount = 8;
        dataPager.PageSize = 12;
        dataPager.UseOnDemandPaging = true;
        dataPager.OnDemandLoading += DataPager_OnDemandLoading;
        base.OnAttachedTo(bindable);
    }

    private void DataPager_OnDemandLoading(object? sender, OnDemandLoadingEventArgs e)
    {
        var source = pagingViewModel!.places!.Skip(e.StartIndex).Take(e.PageSize);
        listView!.ItemsSource = source.AsEnumerable();
    }
}
```

## Requirements to run the demo

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) or [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)
* Xamarin add-ons for Visual Studio (available via the Visual Studio installer).

## Troubleshooting

### Path too long exception

If you are facing path too long exception when building this example project, close Visual Studio and rename the repository to short and build the project.

