## Problemin səbəbi
Fərz edin ki, oyun salonu üçün bir sistem hazırlayırıq və bu sistemdə həm fərqli oyun növləri var(misal: Macəra, Açıq dünya və sair) həm də bu oyunları oynaya biləcəyimiz fərqli cihazlar var(misal: PC, Xbox, PlayStation və sair). Biz isə elə etməliyik ki, müştərinin istədiyi cihazda, seçdiyi oyunu oynaya bilsin. Eləcə də, sistemi elə yaxşı qurmalıyıq ki, gələcəkdə yeni tiplər əlavə edəndə sistem pozulmasın.
## Problemin həlli
Bu problemi həll etmək üçün, Abstract factory dizayn pattern-i tətbiq edə bilərik. Gəlin addım addım Abstract factory dizayn pattern-nini tətbiq edək.
### Addım 1
İlk iş, sistemdə ümumi bir oyun interfeysi təyin edib, daha sonra hər fərqli oyun janrı üçün başqa bir interfeys açmaqdır.
```c#
interface IGame
{
	void Play();
}

interface IShooterGame : IGame
{
	// Gələcəkdə bura, tipə özəl olaraq genişlədilə bilər
}

interface IRacingGame : IGame
{
	// Gələcəkdə bura, tipə özəl olaraq genişlədilə bilər
}
```
### Addım 2
İkinci addımda isə təyin hər bir interfeysin, cihaza özəl konkret növlərini yaratmaqdır.
```c#
class PcShooterGame : IShooterGame
{
	public void Play()
	{
		Console.WriteLine("PC-də shooter oyunu oynanılır");
	}
}

class XboxShooterGame : IShooterGame
{
	public void Play()
	{
		Console.WriteLine("Xbox-da shooter oyunu oynanılır");
	}
}

class PcRacingGame : IRacingGame
{
	public void Play()
	{
		Console.WriteLine("PC-də racing oyunu oynanılır");
	}
}

class XboxRacingGame : IRacingGame
{
	public void Play()
	{
		Console.WriteLine("Xbox-da racing oyunu oynanılır");
	}
}
```
### Addım 3
Növbəti iş olaraq isə, ümumi bir factory interfeysi təyin edib daha sonra, hər cihaz növü üçün factory sinifləri hazırlamalıyıq.
```c#
interface IGameFactory
{
	IShooterGame CreateShooterGame();

	IRacingGame CreateRacingGame();
}

class PCGameFactory
{
	public IShooterGame CreateShooterGame()
	{
		return new PcShooterGame();
	}

	public IRacingGame CreateRacingGame()
	{
		return new PcRacingGame();
	}
}

class XboxGameFactory
{
	public IShooterGame CreateShooterGame()
	{
		return new XboxShooterGame();
	}

	public IRacingGame CreateRacingGame()
	{
		return new XboxRacingGame();
	}
}
```
### Addım 4
Bütün bu əməliyyatları edəndən sonra ortada bir məsələ qalır bəs, proqram hansı factory işlətməli olduğunu hardan biləcək? Bu isə fərqli fərqli yanaşmalarla mümkündür, əgər veb layihəsi yazırsınızsa o zaman `appsettings.json` kimi faylda müəyyən konfiqurasiyalar saxlamaq olar və həmin konfiqurasiyaya uyğun hərəkət etmək və ya daha maraqlı yanaşmaları tətbiq edə bilərsiniz. Test məqsədli sadəcə nəticələri görmək üçün aşağıdakı kodu yoxlaya bilərsiniz:
```c#
Console.OutputEncoding = System.Text.Encoding.UTF8;

var pcFactory = new PcGameFactory();
var xboxFactory = new XboxGameFactory();

var pcShooter = pcFactory.CreateShooterGame();
var xboxRacing = xboxFactory.CreateShooterGame();

pcShooter.Play();
xboxRacing.Play();
```
### Nəticə
![[result.png]]

### Müsbət və mənfi tərəfləri
#### Müsbət tərəfləri ✔
Bizə qazandırdığı əsas müsbət tərəf obyektlərin yaradılmasını mücərrəd hala gətirdik beləliklə obyektləri istifadə edəcək tərəf(client code) obyektlərin yaradılmasından məsuliyyət daşımayacaq. Bu da bizim SOLID prinsiplərinin S-hissəsinə(Single Responsibility Principle) riayət etməyimizə şərait yaratdı. Eləcə də, artıq gələcəkdə cihaz növünü və ya oyun növünü rahatlıqla artıra və ya silə biləcəyimizə görə SOLID O-hissəsinə(Open/Closed Principle) də riayət etmiş olduq. Müsbət cəhətləri SOLID ilə limitli deyil, demək olar həm cihaz həm oyun həm də client code-un bir birilərindən sıx asılılığını da məhv etmiş olduq.
#### Mənfi tərəfləri ❌
Əsas mənfi tərəfi, tətbiq etməsi zaman zaman çətin hala gələ bilir. Xüsusilə, layihə çox kiçikdirsə və gələcəkdə böyüməyəcəksə bu dizayn pattern-i istifadə etmək çox da məsləhət deyil, ifrat mühəndislik etmiş olacaqsınız. Əgər interfeyslər doğru şəkildə bölünməsə gələcəkdə ehtiyac yarandığı təqdirdə ana sinifləri dəyişmək məcburiyyətində qala bilərsiniz.

Müəlliflər: Kamran Niftəliyev və Cəlil Verdiyev
