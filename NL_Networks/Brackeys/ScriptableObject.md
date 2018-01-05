title: Scriptable Objects
date: 2018/1/5 11:51:37 
tag: unity3d

---

* Prefab
* Scriptable Objects
* 利用代码来复制创建多**副本**

<!--more-->

# Scriptable Object #

* 说明：
	* ScriptableObject是一个类，可以让你大量的共享数据独立存储的从脚本实例。不要用类似的命名SerializedObject，这是一个编辑器类和填充不同的目的混淆类。例如考虑您已与拥有一百万整数数组一个脚本prefab。所述阵列占据的存储器4MB并且由预制拥有。每次实例化预制的时候，你会得到该数组的一个副本。如果你创建了10点比赛的对象，那么你最终会在10个实例阵列数据的40MB。
	* 统一序列化所有基本类型，字符串，数组，列出，具体到Unity类型如的Vector3和用Serializable属性作为您的自定义类的副本属于他们被宣布的对象。这意味着，如果你创建了一个个ScriptableObject和存储上万在它声明的整数数组则数组将存储与该实例。这些实例被认为拥有自己的个人数据。个ScriptableObject字段，或任何UnityEngine.Object领域，如MonoBehaviour，网，游戏对象等等，由存储参考通过值而不是。如果你有一个参考个ScriptableObject与百万整数脚本，统一将只存储在脚本数据给个ScriptableObject参考。反过来个ScriptableObject存储阵列。具有到是ScriptableObject，保持4MB数据的基准的预制的10个实例，如在其它示例中所讨论合计为大致4MB和不40MB。
	* 该用途的情况下，使用**ScriptableObject是通过避免值的副本，以减少内存使用**，但你也可以用它来定义可插拔的数据集。这方面的一个例子是想象在RPG游戏中的NPC商店。您可以创建自定义ShopContents个ScriptableObject，每个定义一组可用于购买物品的多个资产。在游戏中有三个区域的场景，每个区可以提供不同层次的项目。你的店铺脚本将引用ShopContents对象，它定义哪些项目是可用的。请参阅示例脚本参考。
	* 一旦你定义是ScriptableObject派生类，你可以使用CreateAssetMenu属性，可以很容易地使用你的类创建自定义的资产。
	* 提示：当在检查员个ScriptableObject引用工作，你可以双击参考字段打开检查你个ScriptableObject。您还可以创建自定义编辑器来定义督察你的类型，以帮助管理它所表示的数据的外观。

## 具体细节 ##

* 创建模版存储的类，并继承**ScriptableObject超类**

`

	//存储脚本的细节类
	public class Card : ScriptableObject
	{
		public int attack;
		public int health;

		public sprite image;
	}

	//此类是显示当前对象的内容，需要添加到对象身上
	public class CardDisplay
	{
		public Card _card;//对信息的引用

		public Text _attack;
		public Text _health;

		public Image _image;

		void Start()
		{
			_attack.text=_card.attack.ToString();
			_health.text=_card.health.ToString();

			_image.Sprite=_card.image;
		}
	}