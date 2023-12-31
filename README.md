**Бесилка**

Windows Forms Project by: 
Filip Boshevski

---
1. Опис на апликацијата
	
	Апликацијата што ја развив е класичната игра Бесилка.

	Со цел да се обезбеди комплетно задоволство кај играчот, имплементирани се и помошни избори кои им покажуваат на играчот одреден број на букви во зборот.

2. Упатство за користењe

	2.1 Нова игра

	![alt text][new_game_screen]

	На почетниот прозорец (слика 1) при стартување на апликацијата имаме можност да започнеме нова игра **(Започни)**.
	После секој успешно погоден збор, на екранот излегува прозорче кое го известува играчот дека победи со што има можност да започне нова игра.

	2.2 Ресетирај

	![alt text][resetiraj]

	Со помош на оваа фунцкионалност, играчот во секој момент од играта може да ја ресетира со што излегува прозорче кое го известува за постоечката игра и потврдува дека сака вистина да ја ресетира играта.

	2.3 Помош избор

	![alt text][pomos_izbor]

	Доколку корисникот има потешкотии во годењето на зборот, може со помош на копчето **(Помош избор)** да открие една рандом буква од зборот.
	Вкупниот број на помошни избори е 2.

	2.4 Правила
	Правилата се едноставни:

	* Зборот треба да се погоди пред да се нацрта целото човече на бесилката.
	* Бројот на грешни избори е 6, односно, 1 за главата, 1 за телото, 1 за секоја рака и 1 за секоја нога.
	* Бројот на помошни избори е 2, со што играчот може да открие една од буквите во зборот.
* Откако една точна или грешна буква се избере таа не може повторно да се избере.

3. Претставување на проблемот

	3.1 Податочни структури

	Главните податоци и [функции](#3.2-) за играта се чуваат во класа ```public class Scene``` во која се чува состојбата на играта во објект од класата ```public class State``` и состојбата за бесилката во објект од класата ```public class Hanger```.

	```c#
	public class Scene
	{
		private State State { get; set; }

        private Besilka Window { get; set; }

        private Pen HangerPen { get; set; }

        private Pen RopePen { get; set; }

        private Hanger Hanger { get; set; }

        private static string Words = "Бесилка,Здраво,Филтер,Мајмун,Јаже,Кајмак,Сирење,Стол,Лепило,Плоча,Картон,Ножица,Стакло,Компјутер,Ризик,Полнач";

        public Scene(Besilka window)
        {
            Window = window;
            State = new State();
            
            InitializeVars();
            AddButtons();
        }

		//Implementacijata na metodite mozhete da ja najdete vo izvorniot kod na klasata

        private void InitializeVars() {}

        public void StartGame() {}

		private void SelectWord() {}

		private void AddLabels() {}

		private void ResetControls() {}

		private void onBtnClick(object sender, EventArgs e) {}

		private void FillChar(char charClicked) {}

		public void GetHelp() {}

		private void ResetStartButton() {}

		private void AddButtons() {}

		public void Draw(Graphics g) {}
	}
	```

	3.1.1 State
	```c#
		public class State
		{
			public State()
			{
				Letters = new List<Label>();
				HangState = HangState.None;
				HelpLeft = 2;
			}

			public List<Label> Letters { get; set; }

			public string Word { get; set; }

			public string EmptyChar => "__";

			public HangState HangState { get; set; }

			public int InitialChoicesLeft => Enum.GetValues(typeof(HangState)).Length - 1;

			public int HelpLeft { get; set; }
		}
	```
	Со оваа класа ја дефинираме состојбата на играта.

	3.1.2 Hanger
	```c#
		public class Hanger
		{
			public Hanger()
			{
				HangerPoints = new Dictionary<string, Point>();
			}

			public Dictionary<string, Point> HangerPoints { get; set; }

			public Point Rope { get; set; }

			public Point Body { get; set; }

			public Point Head { get; set; }

			public Point LeftArm { get; set; }

			public Point RightArm { get; set; }

			public Point LeftLeg { get; set; }

			public Point RightLeg { get; set; }

			public static int HeadDiameter = 40;

			public static int BodyHeight = 85;

			public static int RopeLength = 40;

			public static Point ArmOffset = new Point(30, 50);

			public static Point LegOffset = new Point(40, 40);
		}
	```
	Со оваа класа ја дефинираме состојбата и клучни податоци за бесилката во играта.

	3.2 Алгоритми

	За да биде различна играта секој пат, имплементирано е рандом генерирање на збор од листа на зборови или текст фајл.

	3.2.1 Почетна состојба
		`StartGame();`
		Со повикување на оваа метода прво се проверува дали веќе постои игра во тек, со што се прикажува прозорче за потврдување на нова игра. 

		Потоа се ресетира состојбата на контролите, се одбира рандом збор од листата и се поставуваат цртичките во играта.

	3.2.2 Кликање на буква
		`onBtnClick();`
		На секое копче-буква се додава Event Handler методата onBtnClick која ја зема буквата на прицнатото копче и проверува дали постои во зборот. 

		Доколку постои, ги наоѓа сите инстанци во цртичките и ги заменува со буквата. 

		Во спротивно, ја зголемува состојбата на бесилката за 1, со што кога ќе стигне до десната нога, играта завршува и се прикажува прозорче со порака.

		`FillChar();`
		Оваа метода го прави заменувањето на сите цртички во зборот кои што ја содржат избраната буква. 

		Доколку сите цртички се пополнети, играта завршува и се прикажува прозорче со порака.
			
	3.2.3 Помош избор
		`GetHelp();`
		Со притискање на копчето **(Помош избор)** или мени опцијата **(Помош избор)**, се повикува оваа метода која открива една рандом буква во зборот и ги заменува сите цртички со таа буква.

		Со секое притискање на копчето се намалува бројот на помош избори.

	3.2.3 Цртање на бесилката
		`Draw();`
		Со помош на оваа метода, на екранот се црта бесилката каскадно.

		Прво се иницијализираат сите променливи потребни за цртањето, потоа за секој дел од човечето се проверува индивидуално според состојбата на бесилката и се исцртува истиот.
	3.3 GUI

	За претставување на бесилката искористена е panel контрола со исцртување на компонентите според нејзините димензии како offset.

---
---

[new_game_screen]: https://raw.githubusercontent.com/filipboshevski/besilka/master/Sliki/start_screen.png "Слика 1"
[resetiraj]: https://raw.githubusercontent.com/filipboshevski/besilka/master/Sliki/resetiraj.png "Слика 2"
[pomos_izbor]: https://raw.githubusercontent.com/filipboshevski/besilka/master/Sliki/pomos_izbor.png "Слика 3"