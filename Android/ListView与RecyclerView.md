# RecyclerView：

## 添加依赖：

>```
>implementation 'androidx.recyclerview:recyclerview:1.0.0'
>```

## 定义Adapter

>```java
>public class FruitAdapter extends RecyclerView.Adapter<FruitAdapter.ViewHolder> {
>
>private List<Fruit> mFruitList;
>static class ViewHolder extends RecyclerView.ViewHolder {
>  ImageView fruitImage;
>  TextView fruitName;
>  public ViewHolder(View view) {
>      super(view) ;
>      fruitImage = (ImageView) view.findViewById(R.id.fruit_image);
>      fruitName = (TextView) view.findViewById(R.id.fruit_name);
>  }
>}
>
>public FruitAdapter(List<Fruit> fruitList) {
>  mFruitList = fruitList;
>}
>//重写来自抽象类Adapter的3个方法
>//生成封装有view的ImageView和TextView控件的ViewHolder对象
>@Override
>public ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
>  /*加载item布局*/
>  View view = LayoutInflater.from(parent.getContext( )).inflate(R.layout.fruit_item,parent, false);
>  ViewHolder holder = new ViewHolder(view) ;
>  return holder;
>}
>//设置数据
>@Override
>public void onBindViewHolder(ViewHolder holder, int position){
>  Fruit fruit = mFruitList.get(position) ;
>  holder.fruitImage.setImageResource(fruit. getImageId());
>  holder.fruitName.setText(fruit.getName( ));
>}
>//获得Item数量
>@Override
>public int getItemCount( ) {
>  return mFruitList==null?0:mFruitList.size();
>}
>}
>```

## 在Activity中初始化

>```java
>RecyclerView recyclerView = (RecyclerView) findViewById(R.id.recycler_view);
>/*recyclerView.setLayoutManager(new LinearLayoutManager(this));*/
>LinearLayoutManager layoutManager = new LinearLayoutManager(this);
>recyclerView.setLayoutManager(layoutManager);
>
>FruitAdapter adapter = new FruitAdapter(list);
>recyclerView.setAdapter(adapter);
>```

## 更多RecyclerView

### 设置点击

1.在onCreateViewHolder()方法中注册监听器(在onBindViewHolder中 删除 item后position混乱)

2.可以写一个回调接口，把点击事件的逻辑写到外面

### 获得item的position

```java
holder.getLayoutPosition()
```

### 刷新RecyclerView

```java
adapter.notifyDataSetChanged();//和ListView一样的 插入或删除都可以 但官方不太推荐

adapter.notifyItemInserted(sentenceList.size()-1);//当有item插入时刷新RecyclerView
recyclerView.scrollToPosition(sentenceList.size()-1);//将RecyclerView滚动到最后一个item

adapter.notifyItemRemoved(holder.getLayoutPosition());//通知被删除的地方
adapter.notifyItemRangeChanged(holder.getLayoutPosition(),getItemCount());//从被删除的地方开始刷新
```

### 实现横着滚动

```java
layoutManager.setOrientation(LinearLayoutManager.HORIZONTAL);//默认是VERTICAL
```

### 实现瀑布流布局

```java
 StaggeredGridLayoutManager layoutManager = 
     new StaggeredGridLayoutManager(3,StaggeredGridLayoutManager.VERTICAL);
							//第一个参数为行（列）数 第二个参数为滚动方式
```

### getLayoutPosition 和 getAdapterPosition的区别

[RecyclerView：getLayoutPosition 和 getAdapterPosition - 简书 (jianshu.com)](https://www.jianshu.com/p/83506d1d1ec8)



# ListView：

## (1)基本使用：

>
>
>```java
>public class MainActivity extends AppCompatActivity {
>
>private String[] data = {"...","...",...};
>@Override
>protected void onCreate(Bundle savedInstanceState) {
>   super.onCreate(savedInstanceState);
>   setContentView(R.layout.activity_main);
>   //数据-->适应器-->ListView
>   ArrayAdapter<String> adapter = new ArrayAdapter<String>(
>           MainActivity.this,android.R.layout.simple_list_item_1,data);
>   ListView listView = (ListView) findViewById(R.id.list_view);
>   listView.setAdapter(adapter);
>}
>```

## (2)自定义Adapter：

自定义Adapter类中：

```java
public FruitAdapter(Context context, int textVieResoureId, List<Fruit> objects){
        super(context,textVieResoureId,objects);
        resourceId = textVieResoureId;
    }

    @NonNull
    @Override//getView()方法在每个子项滚进屏幕时调用
    public View getView(int position, @Nullable View convertView, @NonNull ViewGroup parent) {
        //生成相应的Fruit对象
        Fruit fruit = getItem(position);
        //android中布局填充器的实现，即把Xml布局文件解析成View
        //生成Item布局的对象（View对象）
       View view=LayoutInflater.from(getContext()).inflate(resourceId,parent,false);
        //找到Item布局中的ImageView控件
        ImageView fruitImage = (ImageView) view.findViewById(R.id.fruit_image);
        //将相应Item布局的ImageView控件设为相应Fruit的Image
        fruitImage.setImageResource(fruit.getImageId());
        //找到Item布局中的TextView控件
        TextView fruitname =(TextView) view.findViewById(R.id.fruit_name);
        //将相应Item布局的TextView控件设为相应Fruit的name
        fruitname.setText(fruit.getName());
        return view;
    }
}

```

在MainActivity中：

```java
private List<Fruit> fruitList = new ArrayList<>();
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initFruits();//初始化 fruitList 的一个自定义函数
        FruitAdapter adapter = new FruitAdapter(MainActivity.this,R.layout.fruit_item,fruitList);
        ListView listView = (ListView) findViewById(R.id.list_view);
        listView.setAdapter(adapter);
    }
```

其中Fruit类为：

```java
public class Fruit {
    private String name;
    private int imageId;
    public Fruit(String name,int imageId){
        this.name = name;
        this.imageId = imageId;
    }
    public String getName(){
        return name;
    }
    public int getImageId(){
        return imageId;
    }
}
```



## (3)提升ListView的运行效率：

>`getView()`方法在每个子项滚进屏幕时都会被调用，若用户快速滚动，可能会卡
>
>在`getView()`中有一个`convertView`参数，它用于缓存之前加载好的布局
>
>所以可以做以下优化：
>
>```java
>	@NonNull
>@Override
>public View getView(int position, @Nullable View convertView, @NonNull ViewGroup parent) {
>   Fruit fruit = getItem(position);
>
>
>   View view;
>   if (convertView==null){
>       view = LayoutInflater.from(getContext()).inflate(resourceId,parent,false);
>   }else{
>       view=convertView;
>   }
>
>
>   ImageView fruitImage = (ImageView) view.findViewById(R.id.fruit_image);
>   fruitImage.setImageResource(fruit.getImageId());
>   TextView fruitname =(TextView) view.findViewById(R.id.fruit_name);
>   fruitname.setText(fruit.getName());
>   return view;
>}
>```

>
>
>虽然现在已经不会再重复去加载布局，但是每次在getView()方法中还是会调用View的findViewById()方法来获取一次控件的实例。
>
>所以可以再优化：
>
>```java
>@NonNull
>@Override//getView()方法在每个子项滚进屏幕时调用
>public View getView(int position, @Nullable View convertView, @NonNull ViewGroup parent) {
>   Fruit fruit = getItem(position);
>   View view;
>   ViewHolder viewHolder;
>   if ( convertView == null) {
>       view = LayoutInflater.from(getContext()).inflate(resourceId,parent,false);
>       viewHolder = new ViewHolder( ) ;
>       viewHolder.fruitImage = (ImageView) view.findViewById(R.id.fruit_image) ;
>       viewHolder.fruitName(TextView)view.findViewById(R.id.fruit_name);
>       view.setTag(viewHolder); //将ViewHolder存储在View中
>   }else {
>       view = convertView;
>       viewHolder = (ViewHolder) view.getTag();//重新获取ViewHolder
>   }
>   viewHolder.fruitImage.setImageResource(fruit.getImageId());
>   viewHolder.fruitName.setText(fruit.getName( ));
>   return view;
>}
>class ViewHolder {
>   ImageView fruitImage;
>
>   TextView fruitName;
>}
>}
>```
>
>
>
>

### II.ListView的点击事件：

>
>
>```java
>public class MainActivity extends AppCompatActivity {
>
>private List<Fruit> fruitList = new ArrayList<>();
>@Override
>protected void onCreate(Bundle savedInstanceState) {
>   super.onCreate(savedInstanceState);
>   setContentView(R.layout.activity_main);
>   initFruits();
>   FruitAdapter adapter = new FruitAdapter(MainActivity.this,R.layout.fruit_item,fruitList);
>   ListView listView = (ListView) findViewById(R.id.list_view);//***
>   listView.setAdapter(adapter);
>   //***
>   listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
>       @Override
>       public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
>           Fruit fruit = fruitList.get(position);
>           Toast.makeText(MainActivity.this,fruit.getName(),Toast.LENGTH_SHORT).show();
>       }
>   });
>   //***
>}
>```



### III.ListView的缺点：

>> 如果不使用一些技巧来提升它的运行效率，那么ListView的性能就会非常差;
>
>> 还有,ListView的扩展性也不够好，它只能实现数据**纵向滚动**的效果，如果我们想实现横向滚动的话，LislView是做不到的。

