# 一、基本使用

## View Binding所需环境

- 首先需要将Android Studio升级到3.6版本及以上
- 升级Gradle plugin版本到3.6.1
- 并build.gradle中手动开启view binding

```
android{
	//老版本
    viewBinding {
        enabled = true
    }
    //新版本
    buildFeatures {
        viewBinding = true
    }

}
```

生成绑定类时忽略某个布局文件，请将 `tools:viewBindingIgnore="true"` 属性添加到相应布局文件的根视图中：
```xml
<LinearLayout
              ...
              tools:viewBindingIgnore="true" >
    		  ...
</LinearLayout>
```


## 1.在Activity中使用

例：在MainActivity中

```java
public class MainActivity extends AppCompatActivity {
    ActivityMainBinding mainBinding;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        mainBinding = ActivityMainBinding.inflate(getLayoutInflater());
        
        //setContentView(R.layout.activity_main);//将这个修改为如下
        setContentView(mainBinding.getRoot());
        
        mainBinding.test.setText("This is a TextView");//其中 test 为TextView的id
    }
}
```

## 2.在Fragment中使用

例：在BlankFragment中

```java
public class BlankFragment extends Fragment {
    
    private FragmentBlankBinding binding;
    
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        binding=FragmentBlankBinding.inflate(inflater,container,false);
        return binding.getRoot();
    }
}
```

## 3.在RecyclerView.Adapter中

```java
public static class ViewHolder extends RecyclerView.ViewHolder{
    
    public ViewHolder(ItemBinding itemBinding) {
        super(itemBinding.getRoot());
    }
    
    @NonNull
	@Override
	public ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
    	ItemBinding binding = ItemBinding.
        	inflate(LayoutInflater.from(parent.getContext()), parent, false);
    	return new ViewHolder(binding);
	}
}
```

# 二、更优雅用法

依托于基类

## Activity

```java
abstract public class BaseActivity<VB extends ViewBinding> extends AppCompatActivity {
    private VB mBinding;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        mBinding= ViewBindingUtil.inflateWithGeneric(this,getLayoutInflater());
        setContentView(mBinding.getRoot());
    }
    
    public VB getBinding() {
        return mBinding;
    }
}
```

```java
class MainActivity extends BaseBindingActivity<ActivityMainBinding> {
 
  @Override
  protected void onCreate(@Nullable Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    getBinding().textView.setText("This is a TextView");
  }
}
 
```

## Fragment

