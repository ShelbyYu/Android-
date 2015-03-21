# Android-
Android自定义控件的属性配置


    在Android的xml布局文件里，xmlns:android="http://schemas.android.com/apk/res/android"，就是定义了xml的命名空间，以android开头，比如：android：id="@+id/i_am"

    有时候，我们也想自定义命名空间。比如，在xmlns:android="http://schemas.android.com/apk/res/android"的下面写上：xmlns:app="http://schemas.android.com/apk/res-auto"。以后就可以在整个xml布局文件里的控件下面设置属性，例如：app:me="@drawable/is_me"。但是，这个属性必须在res/values/attrs.xml里面声明过：

<declare-styleable name="isme">
     <attr name="me" format="reference" />
</declare-styleable>

    属性设置好了，但是怎么在java代码里面获取到这些控件的属性呢。



 1 public CustomComponent(Context context, AttributeSet attrs) {
 2     super(context, attrs);
 3 
 4     TypedArray a = context.obtainStyledAttributes(attrs,R.styleable.CusComponent);
 5 
 6     int imageSrcId;
 7 
 8     try {
 9          imageSrcId = a.getResourceId(R.styleable.isme_me,R.drawable.myimage);
10 
11     } finally {
12          a.recycle();
13     }
14     LayoutInflater inflater = LayoutInflater.from(context);
15 
16     inflater.inflate(R.layout.custom_component_layout, this, true); // 给自定义控件设置布局
17     b = (ImageButton)findViewById(R.id.btn); // 获取到布局上的ImageButton
18     b.setImageResource(imageSrcId);
19 
20 }

     还有一点，就是命名空间的问题，就是res与res-auto的区别。通常我们在布局文件中使用自定义属性的时候 会这样写 xmlns:app=http://schemas.android.com/apk/res-auto,但如果你当前工程是做为lib使用，那么你如上所写 ，会出现找不到自定义属性的错误 。 这时候你就必须 写成 xmlns:app=http://schemas.android.com/apk/res/包名路径。

       如果自定义属性出现报错的话，例如这样的错误：Unexpected namespace prefix "app" found for tag fragment。解决方法：

到Eclipse的problems 标签找到这个错误，右键然后选择quick fix菜单，在弹出的菜单中选择不检查这个文件（Disable check in this file only）就可以了。
