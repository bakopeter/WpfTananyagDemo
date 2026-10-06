md_content = """# WPF Lépésről Lépésre Útmutató

Ez a részletes útmutató bemutatja, hogy mibe mit kell tenned, és mit hogyan kell beállítanod a tananyag összes eleménél.

---

## 1. Lépés: Projekt előkészítése a Visual Studio-ban

1. Nyisd meg a Visual Studio-t.
2. Kattints a **Create a new project** (Új projekt létrehozása) gombra.
3. Keresd meg a **WPF Application (.NET)** sablont (C# nyelven), majd kattints a **Next** gombra.
4. Adj neki egy nevet (pl. `WpfTananyagDemo`), majd kattints a **Create** gombra.
5. Megnyílik a fejlesztői felület. A képernyő közepén látod a `MainWindow.xaml` fájlt.

> **Alapfogalmak:**
> * A WPF lényege, hogy elválasztja a C# üzleti logikát a XAML felülettől.
> * A felületet a XAML nevű, XML alapú nyelvvel írjuk le.
> * Nem pixelenként pozicionálunk, hanem reszponzív tárolókat építünk egymásba.

---

## 2. Lépés: A legkülső tároló – A felület váza (DockPanel)

* **Mibe tesszük?** A `<Window>` tag-en belülre helyezzük el a `DockPanel`-t.
* **Miért?** A `DockPanel` a szélekhez igazítja az elemeket (`Top`, `Bottom`, `Left`, `Right`), az utolsó eleme pedig kitölti a középen maradó teljes helyet.
* **Mit állítunk be a DockPanel-en?**
  * `LastChildFill="True"`: Ez gondoskodik róla, hogy a benne lévő utolsó elem (a `TabControl`) automatikusan kitöltse a fennmaradó helyet.

### Mit teszünk a DockPanel-be (sorrendben)?

1. **Menu** (Tetejére)
   * Beállítás: `DockPanel.Dock="Top"`
2. **ToolBar** (E alá, a tetejére)
   * Beállítás: `DockPanel.Dock="Top"`
3. **StatusBar** (Az ablak aljára)
   * Beállítás: `DockPanel.Dock="Bottom"`
   * A `StatusBar`-ba teszünk egy `TextBlock`-ot (állapotjelző szöveg) és egy `ProgressBar`-t (folyamatjelző csík, pl. `Value="45"`).
4. **TabControl** (Középre, ez az utolsó elem)
   * Mivel ez az utolsó elem, automatikusan elfoglalja a teljes középső részt.

---

## 3. Lépés: A TabControl füleinek és tartalmának felépítése

A `TabControl`-on belül `TabItem` elemeket (füleket) hozunk létre.

---

### 1. FÜL: Layoutok és Elrendezések (`TabItem Header="Layoutok"`)

* **Mibe tesszük a fülek tartalmát?** Egy `Grid` (rács) tárolóba.

#### a) A Grid sorainak és oszlopainak beállítása (`RowDefinitions`, `ColumnDefinitions`)
Nyisd meg a `Grid.RowDefinitions` és `Grid.ColumnDefinitions` tag-eket:

* **Oszlopok:**
  * 0. oszlop: `<ColumnDefinition Width="*"/>` (arányos szélesség)
  * 1. oszlop: `<ColumnDefinition Width="2*"/>` (kétszer olyan széles, mint az első)
* **Sorok:**
  * 0. sor: `<RowDefinition Height="Auto"/>` (tartalomhoz igazodó magasság)
  * 1. sor: `<RowDefinition Height="100"/>` (fix 100 DIP / eszközfüggetlen pixel magasság)
  * 2. sor: `<RowDefinition Height="*"/>` (fennmaradó hely elosztása)

#### b) Elemek elhelyezése a Grid celláiban:

* **StackPanel** a 0. sor 0. oszlopába:
  * Beállítás: `Grid.Row="0"` `Grid.Column="0"` (alapértelmezés szerint a 0. sor/oszlop az alap).
  * Tegyünk bele `TextBlock`-ot és `Button`-okat. A `StackPanel` egymás alá halmozza őket.
* **WrapPanel** a 0. sor 1. oszlopába:
  * Beállítás: `Grid.Row="0"` `Grid.Column="1"`.
  * Saját tulajdonságok beállítása: `Orientation="Horizontal"`, `ItemWidth="100"`, `ItemHeight="40"`.
  * Tegyünk bele gombokat (`Button`)! Ha nem férnek el egy sorban, a `WrapPanel` automatikusan új sorba töri őket.
* **Canvas** az 1. sorba, amely átéri mindkét oszlopot:
  * Beállítás: `Grid.Row="1"` `Grid.Column="0"` `Grid.ColumnSpan="2"` (összevonunk 2 oszlopot, mint a CSS-ben).
  * A `Canvas`-on belül abszolút pozicionálást használunk:
    * `Rectangle` (Téglalap): `Canvas.Left="20"`, `Canvas.Top="20"`, `Panel.ZIndex="1"`.
    * `Ellipse` (Kör/Ellipszis): `Canvas.Left="50"`, `Canvas.Top="30"`, `Panel.ZIndex="2"` (a ZIndex miatt ez lesz felül).
* **GroupBox** (Általános elrendezési tulajdonságok bemutatása) a 2. sorba:
  * Beállítás: `Grid.Row="2"` `Grid.Column="0"`.
  * Tegyünk bele gombokat, és állítsuk be a tananyag szerinti tulajdonságokat:
    * `Margin="5"` (külső margó)
    * `Padding="5"` (belső térköz)
    * `HorizontalAlignment="Left"` / `"Center"` / `"Stretch"` (vízszintes igazítás)
    * `VerticalAlignment="Center"` (függőleges igazítás)
    * `Width="120"`, `Height="30"`, `MinHeight="20"`, `MaxHeight="50"` (méretkorlátok)

---

### 2. FÜL: Tartalom és Szöveges Vezérlők (`TabItem Header="Tartalom & Szöveg"`)

* **Mibe tesszük?** Egy `ScrollViewer`-be, hogy ha nem fér el a tartalom a képernyőn, automatikusan megjelenjen a görgetősáv. A `ScrollViewer`-en belül egy `StackPanel`-t használunk az elemek függőleges halmozására.

#### Mit teszünk ide?

* **GroupBox (Content Controls - Tartalmi vezérlők):**
  * **Label:** `Content="Ez egy Label felirat"`. Állítsunk be hozzá ToolTip-et (`ToolTip="Súgó szöveg"`)!
  * **Button:** `Content="Kattints rá"`.
  * **CheckBox:** `Content="Jelölőnégyzet"` `IsChecked="True"`.
  * **RadioButton:** `Content="Választógomb 1"` (hozzunk létre kettőt belőle).
  * **ToggleButton:** `Content="Be/Ki gomb"`.
  * **RepeatButton:** `Content="Nyomva tartós gomb"`.
  * **Expander:** `Header="Kattints a lenyitáshoz"`. A belsejébe tegyünk egy `TextBlock`-ot! Összecsukható/lenyitható panelt képez.

* **GroupBox (Text Controls - Szöveges vezérlők):**
  * **TextBlock:** `Text="Írásvédett szöveg (gyors, könnyű súlyú)"`.
  * **TextBox:** `Text="Szerkeszthető szövegmező"`.
  * **PasswordBox:** Maszkolt jelszóbeviteli mező.
  * **RichTextBox:** Formázott szövegszerkesztő. Ezen belül `<FlowDocument><Paragraph><Bold>Formázott szöveg</Bold></Paragraph></FlowDocument>` szerkezetet használunk.

---

### 3. FÜL: Elemgyűjtemények és Állapotjelzők (`TabItem Header="Elemek & Állapotok"`)

* **Mibe tesszük?** Egy `Grid`-be, amely 2 oszlopra van osztva (`<ColumnDefinition Width="*"/>` kétszer).

#### Bal oldali oszlop (Item Controls - Elemgyűjtemények)
* **ComboBox** (`Grid.Column="0"`): Legördülő listamező, belsejében `<ComboBoxItem Content="Elem 1"/>` elemekkel.
* **ListBox:** Lista elem.
  * **ContextMenu hozzáadása:** A `ListBox`-on belül hozzunk létre egy `ListBox.ContextMenu`-t, benne `MenuItem`-ekkel (jobb klikkes helyi menü)!
* **ListView:** Listanézet vezérlő (`ListViewItem` elemekkel).
* **TreeView:** Faalapú elrendezésű lista (`TreeViewItem` elemekkel, amelyek egymásba ágyazhatók).
* **DataGrid:** Komplex táblázatos adatszerkesztő és megjelenítő.

#### Jobb oldali oszlop (Intervallumok és Dátumok)
* **Slider:** Csúszka érték kiválasztásához. Beállítások: `Minimum="0"`, `Maximum="100"`, `Value="50"`.
* **ScrollBar:** Közvetlen görgetősáv (ritkán használjuk közvetlenül, mert a `ScrollViewer` tartalmazza).
* **DatePicker:** Dátumválasztó vezérlő naptár-előugróval.
* **Calendar:** Beágyazott naptár vezérlő.

---

### 4. FÜL: Speciális és Média Vezérlők (`TabItem Header="Speciális & Média"`)

* **Mibe tesszük?** Egy 2x2-es `Grid`-be (`Grid.RowDefinitions` és `Grid.ColumnDefinitions`).

#### Mit hova teszünk?

* **Viewbox & Shapes (0. sor, 0. oszlop):**
  * **Viewbox:** Automatikusan átméretezi a benne lévő tartalmat.
  * Belsejébe tegyünk egy `StackPanel`-t, abba pedig `Shape` (alakzat) elemeket:
    * `<Rectangle Fill="Red" Height="30" Width="30"/>`
    * `<Ellipse Fill="Blue" Height="30" Width="30"/>`
    * `<Line Stroke="Black" X1="0" X2="30" Y1="0" Y2="30"/>`
    * `<Path Data="M 0,20 L 20,0 L 40,20 Z" Fill="Green"/>`
    * `<Separator/>` (elválasztó vonal).

* **InkCanvas (0. sor, 1. oszlop):**
  * **InkCanvas:** Tollal vagy egérrel történő szabadkézi rajzoláshoz. Állítsunk be neki hátteret: `Background="LightYellow"`.

* **Média (1. sor, 0. oszlop):**
  * **Image:** Kép megjelenítésére szolgál (`Source="kep.png"`).
  * **MediaElement:** Hang- és videolejátszási vezérlő (`LoadedBehavior="Manual"`).

* **WebBrowser (1. sor, 1. oszlop):**
  * **WebBrowser:** Webes tartalom beágyazására szolgáló komponens.

---

## 4. Lépés: A kész XAML kód

*(Másold be a `MainWindow.xaml` fájlba!)*
"""

file_path = "WPF_Tananyag_Utmutato.md"
with open(file_path, "w", encoding="utf-8") as f:
    f.write(md_content)

print(f"File created: {file_path}")
