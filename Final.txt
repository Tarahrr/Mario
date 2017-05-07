#include <SFML/Graphics.hpp>
#include<iostream>
#include <fstream>
#include <string>


 //VARIABLE
char ** map;
int width = 0, length =0;
using namespace std;
int d = 20 , k = 0  ,Coin  = 0 ;
bool EndGame = false; 
bool OVER = false;
////////////////////////Scoot//////////////////////////////////////////////////
class Scoot
{
public:
	int * ScootX;
	int * ScootY;
	int * MoodScoot;
	int * ClockScoot;
	bool * Coinfront;
	int numScoot;
	sf::Sprite ScootSprite;
	sf::Texture ScootTexture;

	Scoot::Scoot(int NumKoopa)
	{
		numScoot = NumKoopa;
		ScootX = new int [numScoot];
		ScootY = new int [numScoot];
		MoodScoot = new int [numScoot];
		ClockScoot = new int [numScoot];
		Coinfront = new bool [numScoot];
		if(!ScootTexture.loadFromFile("C:/Users/User/Documents/Visual Studio 2010/Projects/Mario Game/Mario Game/Pictures/kik.png"))
			cout<<"cant load a Scoot\n";
		ScootSprite.setTexture(ScootTexture);

		for(int i = 0 ; i < numScoot ; i ++)
		{
			ScootX [i] = -1;
			ScootY[i]= -1 ;
			MoodScoot[i]= -1 ;
			ClockScoot [i] = 0;
			Coinfront[i] = false;
		}
	}
	void Scoot::Move(char ** map , int length , int width)
	{	
		for(int i = 0 ; i < numScoot ; i ++)
		{
			if(ScootX[i] != -1 && ScootY[i]!= -1  && ClockScoot[i] >= 2) // if clock
			{
				ClockScoot[i] = 0;
				if(MoodScoot[i] == 3)
				{
					if((map[ScootX[i]][ScootY[i]+1] == '.' || map[ScootX[i]][ScootY[i]+1] == '?' || map[ScootX[i]][ScootY[i]+1] == 's' || map[ScootX[i]][ScootY[i]+1] == 'K' || map[ScootX[i]][ScootY[i]+1] == 'k' || map[ScootX[i]][ScootY[i]+1] == 'g' || map[ScootX[i]][ScootY[i]+1] == 'G')&& map[ScootX[i]+1][ScootY[i]+1] != '.')
					{
						if(map[ScootX[i]][ScootY[i]+1] != 's')
						{
							if(map[ScootX[i]][ScootY[i]+1] == '?')
							{
								Coinfront[i] = true;
								map[ScootX[i]][ScootY[i]]= '.';
								ScootY[i]++;
							}
							else
							{
								if(Coinfront[i] == true)
								{ 
									Coinfront[i] = false;
									map[ScootX[i]][ScootY[i]+1]= 'G';
									ScootY[i]++;
								}
								else
								{
									map[ScootX[i]][ScootY[i]]= '.';
									map[ScootX[i]][ScootY[i]+1]= 'G';
									ScootY[i]++;
								}
							}
						}
						else
						{
							EndGame = true;
							OVER =true;
						}
					}
					else if(map[ScootX[i]][ScootY[i]+1] == '#' || map[ScootX[i]][ScootY[i]+1] == 't' ||map[ScootX[i]][ScootY[i]+1] == 'c' || ScootY[i] == length-1)
					{
						MoodScoot[i] = 2;
					}
					else if(map[ScootX[i]][ScootY[i]+1]=='.' && map[ScootX[i]+1][ScootY[i]+1]=='.')
					{
						map[ScootX[i]][ScootY[i]] = '.';
						for(int h=ScootX[i] ; h < width ;h++)
						{
							if(h == width - 1 )
							{
								map[ScootX[i]][ScootY[i]] = '.';
								ScootX[i] = ScootY[i] = -1;
								break;
							}	
							if(map[h][ScootY[i]+1] != '.')
							{
								map[h-1][ScootY[i]+1] = 'Y';
								ScootX[i] = h -1;
								ScootY[i]++;
								break;
							}
						}
					}
				}
				else if(MoodScoot[i] == 2 )
				{
					if(( map[ScootX[i]][ScootY[i]-1] == '.' || map[ScootX[i]][ScootY[i]-1] == '?' ||  map[ScootX[i]][ScootY[i]-1] == 's' || map[ScootX[i]][ScootY[i]-1] =='K' || map[ScootX[i]][ScootY[i]-1] == 'k' || map[ScootX[i]][ScootY[i]-1] =='g' || map[ScootX[i]][ScootY[i]-1] =='G' )&& map[ScootX[i]+1][ScootY[i]-1] != '.' ) 
					{
						if(map[ScootX[i]][ScootY[i]-1] != 's')
						{
							if(map[ScootX[i]][ScootY[i]-1] == '?')
							{
								Coinfront[i] = true;
								map[ScootX[i]][ScootY[i]]= '.';
								ScootY[i]--;
							}
							else
							{
								if(Coinfront[i] == true)
								{ 
									Coinfront[i] = false;
									map[ScootX[i]][ScootY[i]-1]= 'G';
									ScootY[i]--;
								}
								else
								{
									map[ScootX[i]][ScootY[i]]= '.';
									map[ScootX[i]][ScootY[i]-1]= 'G';
									ScootY[i]--;
								}
							}
						}
						else
						{
							EndGame = true;
							OVER =true;
						}	
					}
					else if( map[ScootX[i]][ScootY[i]-1] == '#' || map[ScootX[i]][ScootY[i]-1] == 't'|| map[ScootX[i]][ScootY[i]-1] == 'c' || ScootY[i] == 0)
					{			
						MoodScoot[i] = 3;
					}
					else if(map[ScootX[i]][ScootY[i]-1]=='.' && map[ScootX[i]+1][ScootY[i]-1]=='.')
					{
						map[ScootX[i]][ScootY[i]] = '.';
						for(int h=ScootX[i] ; h < width ;h++)
						{
							if(h == width - 1 )
							{
								map[ScootX[i]][ScootY[i]] = '.';
								ScootX[i] = ScootY[i] = -1;
								break;
							}	
							if(map[h][ScootY[i]-1] != '.')
							{
								map[h-1][ScootY[i]-1] = 'y';
								ScootX[i] = h -1;
								ScootY[i]--;
								break;
							}
						}
					}
				}
			}
		}
	}
};	
////////////////////////Class For Coin & End //////////////////////////////////
class Others
{
public:
	int LoopCoin;
	sf::Vector2i End;
	sf::Texture CoinTexture  ,BoxTexture , EndTexture, kikboxTexture ;
	sf::Sprite CoinSprite, BoxSprite , Endsprite , kikBoxSprite;
	sf::Font font;
	sf::Text TimeText , CoinText , PlayerText , CText , TText ;
	string FileName;
	// Constructor
	Others::Others(int width)
	{	
		// font
		if (!font.loadFromFile("GOUDYSTO.ttf"))
			std::cout<<"CANT LOAD FONT \n";
		// Text
		CText.setFont(font);
		CText.setCharacterSize(15);
		CText.setColor(sf::Color::Yellow);
		CText.setPosition( width*32 - 260 ,40);

		CoinText.setFont(font);
		CoinText.setCharacterSize(20);
		CoinText.setColor(sf::Color::Yellow);
		CoinText.setPosition( width*32 - 140 ,40);

		TText.setFont(font);
		TText.setCharacterSize(20);
		TText.setColor(sf::Color::White);
		TText.setPosition( width*32 - 300,10);

		TimeText.setFont(font);
		TimeText.setCharacterSize(25);
		TimeText.setColor(sf::Color::White);
		TimeText.setPosition( width*32 - 140 ,10);

		CoinText.setColor(sf::Color::Yellow);
		TimeText.setColor(sf::Color::White);
		
		End.x = End.y = -1;
		LoopCoin = 0;
		FileName = "";
		
		//sprite and texture
		if(!EndTexture.loadFromFile("C:/Users/User/Documents/Visual Studio 2010/Projects/Mario Game/Mario Game/Pictures/END.png"))
			cout<<"cant load a End\n";
		Endsprite.setTexture(EndTexture);

		if(!kikboxTexture.loadFromFile("C:/Users/User/Documents/Visual Studio 2010/Projects/Mario Game/Mario Game/Pictures/KikBOX.png"))
			cout<<"cant load a End\n";
		kikBoxSprite.setTexture(kikboxTexture);

		if(!BoxTexture.loadFromFile("C:/Users/User/Documents/Visual Studio 2010/Projects/Mario Game/Mario Game/Pictures/Box.png"))
			cout<<"cant load Box \n";
		BoxSprite.setTexture(BoxTexture);

		if(!CoinTexture.loadFromFile("C:/Users/User/Documents/Visual Studio 2010/Projects/Mario Game/Mario Game/Pictures/Coin.png"))
				cout<<"cant load a Coin \n";
		CoinSprite.setTexture(CoinTexture);
	}

	// Animation o f coin
	void Coinanimation (int num)
	{
		FileName= "C:/Users/User/Documents/Visual Studio 2010/Projects/Mario Game/Mario Game/Pictures/CionAnimation.png";

		int ans = num % 4;
		if(ans == 0)
		{
			if(!CoinTexture.loadFromFile(FileName))
				cout<<"cant load a Coin Animation \n";
			CoinSprite.setTextureRect(sf::IntRect(ans*32,0,32,32));
		}
		else if(ans == 1)
		{
			if(!CoinTexture.loadFromFile(FileName))
				cout<<"cant load a Coin Animation \n";
			CoinSprite.setTextureRect(sf::IntRect(ans*32,0,32,32));
		}
		else if(ans == 2)
		{
			if(!CoinTexture.loadFromFile(FileName))
				cout<<"cant load a Coin Animation \n";
			CoinSprite.setTextureRect(sf::IntRect(ans*32,0,32,32));
		}
		else if(ans == 3)
		{
			if(!CoinTexture.loadFromFile(FileName))
				cout<<"cant load a Coin Animation \n";
			CoinSprite.setTextureRect(sf::IntRect(ans*32,0,32,32));
		}
		else
		{
			if(!CoinTexture.loadFromFile(FileName))
				cout<<"cant load a Coin Animation \n";
			CoinSprite.setTextureRect(sf::IntRect(0,0,32,32));
		}
	}
};
///////////////////////////////////////////////////////////////Gooomba////////////////////////////////////////////////////////////////
class Goomba
{
public:
	int * GoombaX;
	int * GoombaY;
	int * Mood;
	int * GoombaClock;
	int numGoomba;
	int  *loopGoomba;
	bool *  Coinfront;
	string FileName;
	sf::Texture gtexture ;
	sf::Sprite gsprite ;

	//Constructor
	Goomba::Goomba(int length,int width,char ** map)
	{
		FileName = "C:/Users/User/Documents/Visual Studio 2010/Projects/Mario Game/Mario Game/Pictures/Goomba.png";	
		if(!gtexture.loadFromFile("C:/Users/User/Documents/Visual Studio 2010/Projects/Mario Game/Mario Game/Pictures/goomba 1.png"))
			cout<<"cant load a Goomba \n";
		gsprite.setTexture(gtexture);

		numGoomba =0;
	//	loopGoomba =0;
		for(int i = 0 ;i<width;i++)
			for(int j = 0 ; j<length;j++)
				if(map[i][j] == 'g' || map[i][j] == 'G')
					numGoomba++;
		//
		GoombaX = new int[numGoomba]; // -1 is null
		GoombaY = new int[numGoomba];// -1 is null
		Mood = new int[numGoomba]; // 0 is for g and 1 is for G 0 is null
		loopGoomba = new int[numGoomba];
		 GoombaClock = new int[numGoomba];
		 Coinfront = new bool [numGoomba];
		for(int i = 0 ;i<numGoomba;i++)
		{
			GoombaX[i] = -1;
			GoombaY[i] = -1;
			Mood[i] = -1;
			 GoombaClock[i] = 0;
			 loopGoomba[i] = 0;
			 Coinfront [i] = false;
		}
		int q = 0;
		for(int i = 0 ;i<width;i++)
		{
			for(int j = 0 ; j<length;j++)
				if(map[i][j] == 'g')
				{
					GoombaX[q] = i;
					GoombaY[q] = j;
					Mood[q] = 0;
					q++;
				}
				else if(map[i][j] == 'G')
				{
					GoombaX[q] = i;
					GoombaY[q] = j;
					Mood[q] = 1;
					q++;
				}
		}
	}
	// animation of Goomba
	void AnimationGoomba(int num)
	{
		int ans = num % 2;
		if(ans == 0)
		{
			if(!gtexture.loadFromFile(FileName))
				cout<<"cant load a Goombaanimation \n";
			gsprite.setTextureRect(sf::IntRect(ans*32,0,32,32));
		}
		else if(ans == 1)
		{
			if(!gtexture.loadFromFile(FileName))
				cout<<"cant load a Goombaanimation \n";
			gsprite.setTextureRect(sf::IntRect(ans*32,0,32,32));
		}
		else
		{
			if(!gtexture.loadFromFile(FileName))
				cout<<"cant load a Goombaanimationer \n";
			gsprite.setTextureRect(sf::IntRect(ans*32,0,32,32));
		}
	}
	// move Goomaba
	void MoveGoomba(char ** map , int length , int width)
	{
		
		for(int i = 0 ; i < numGoomba ; i ++)
		{
			if(GoombaX[i] != -1 && GoombaY[i]!= -1  && GoombaClock[i] >= 25) // if clock
			{
				GoombaClock [i] = 0;
				loopGoomba[i]++;
				if(Mood[i] == 1)// G  move rigth
				{
					if((map[GoombaX[i]][GoombaY[i]+1] == '.' || map[GoombaX[i]][GoombaY[i]+1] == '?' || map[GoombaX[i]][GoombaY[i]+1] == 's' || map[GoombaX[i]][GoombaY[i]+1] == 'K' || map[GoombaX[i]][GoombaY[i]+1] == 'k' || map[GoombaX[i]][GoombaY[i]+1] == 'g' || map[GoombaX[i]][GoombaY[i]+1] == 'G')&& map[GoombaX[i]+1][GoombaY[i]+1] != '.')
					{
						if(map[GoombaX[i]][GoombaY[i]+1] != 's')
						{
							if(map[GoombaX[i]][GoombaY[i]+1] == '?')
							{
								Coinfront[i] = true;
								map[GoombaX[i]][GoombaY[i]]= '.';
								GoombaY[i]++;
							}
							else
							{
								if(Coinfront[i] == true)
								{ 
									Coinfront[i] = false;
									map[GoombaX[i]][GoombaY[i]+1]= 'G';
									GoombaY[i]++;
								}
								else
								{
									map[GoombaX[i]][GoombaY[i]]= '.';
									map[GoombaX[i]][GoombaY[i]+1]= 'G';
									GoombaY[i]++;
								}
							}
						}
						else
						{
							EndGame = true;
							OVER =true;
						}
					}
					else if(map[GoombaX[i]][GoombaY[i]+1] == '#' || map[GoombaX[i]][GoombaY[i]+1] == 't' ||map[GoombaX[i]][GoombaY[i]+1] == 'c' || GoombaY[i] == length-1)
					{
						Mood[i] = 0;
					}
					else if(map[GoombaX[i]][GoombaY[i]+1]=='.' && map[GoombaX[i]+1][GoombaY[i]+1]=='.')
					{
						map[GoombaX[i]][GoombaY[i]] = '.';
						for(int h=GoombaX[i] ; h < width ;h++)
						{
							if(h == width - 1 )
							{
								map[GoombaX[i]][GoombaY[i]] = '.';
								GoombaX[i] = GoombaY[i] = -1;
								break;
							}	
							if(map[h][GoombaY[i]+1] != '.')
							{
								map[h-1][GoombaY[i]+1] = 'g';
								GoombaX[i] = h -1;
								GoombaY[i]++;
								break;
							}
						}
					}
				}
				else if(Mood[i] == 0 ) // g left
				{
					if(( map[GoombaX[i]][GoombaY[i]-1] == '.' || map[GoombaX[i]][GoombaY[i]-1] == '?' ||  map[GoombaX[i]][GoombaY[i]-1] == 's' || map[GoombaX[i]][GoombaY[i]-1] =='K' || map[GoombaX[i]][GoombaY[i]-1] == 'k' || map[GoombaX[i]][GoombaY[i]-1] =='g' || map[GoombaX[i]][GoombaY[i]-1] =='G' )&& map[GoombaX[i]+1][GoombaY[i]-1] != '.' ) 
					{
						if(map[GoombaX[i]][GoombaY[i]-1] != 's')
						{
							if(map[GoombaX[i]][GoombaY[i]-1] == '?')
							{
								Coinfront[i] = true;
								map[GoombaX[i]][GoombaY[i]]= '.';
								GoombaY[i]--;
							}
							else
							{
								if(Coinfront[i] == true)
								{ 
									Coinfront[i] = false;
									map[GoombaX[i]][GoombaY[i]-1]= 'g';
									GoombaY[i]--;
								}
								else
								{
									map[GoombaX[i]][GoombaY[i]]= '.';
									map[GoombaX[i]][GoombaY[i]-1]= 'g';
									GoombaY[i]--;
								}
							}
						}
						else
						{
							EndGame = true;
							OVER =true;
						}
					}
					else if( map[GoombaX[i]][GoombaY[i]-1] == '#' || map[GoombaX[i]][GoombaY[i]-1] == 't'|| map[GoombaX[i]][GoombaY[i]-1] == 'c' || GoombaY[i] == 0)
					{			
						Mood[i] = 1;
					}
					else if(map[GoombaX[i]][GoombaY[i]-1]=='.' && map[GoombaX[i]+1][GoombaY[i]-1]=='.')
					{
						map[GoombaX[i]][GoombaY[i]] = '.';
						for(int h=GoombaX[i] ; h < width ;h++)
						{
							if(h == width - 1 )
							{
								map[GoombaX[i]][GoombaY[i]] = '.';
								GoombaX[i] = GoombaY[i] = -1;
								break;
							}	
							if(map[h][GoombaY[i]-1] != '.')
							{
								map[h-1][GoombaY[i]-1] = 'g';
								GoombaX[i] = h -1;
								GoombaY[i]--;
								break;
							}
						}
					}
				}
				AnimationGoomba(loopGoomba[i]);
			}
		}
	}

};
////////////////////////Koopa Class////////////////////////////////////////////
class Koopa
{
public:
	int * KoopaX;
	int * KoopaY;
	int * Mood;
	int * KoopaMove;
	int * KoopaKik;
	bool * Coinfront;
	int numKoopa;
	int * loopKoopa;
	string FileName;
	sf::Texture koopaRighttexture  ,koopaLefttexture  , kikTexture ;
	sf::Sprite koopaRightsprite , koopaLeftsprite  , KikSprite ;
	sf::Clock KoopaCloSck;

	// Constructor
	Koopa::Koopa(int length,int width,char ** map)
	{
		FileName = "";
	if(!kikTexture.loadFromFile("C:/Users/User/Documents/Visual Studio 2010/Projects/Mario Game/Mario Game/Pictures/kik.png"))
				cout<<"cant load  koopa kik\n";
		KikSprite.setTexture(kikTexture);
	
		if(!koopaRighttexture.loadFromFile("C:/Users/User/Documents/Visual Studio 2010/Projects/Mario Game/Mario Game/Pictures/RightKoopa.png"))
				cout<<"cant load koopa animation \n";
		koopaRightsprite.setTexture(koopaRighttexture);

		if(!koopaLefttexture.loadFromFile("C:/Users/User/Documents/Visual Studio 2010/Projects/Mario Game/Mario Game/Pictures/LeftKoopa.png"))
			cout<<"cant load koopa animation \n";
		koopaLeftsprite.setTexture(koopaLefttexture);
		numKoopa =0;
		 // counting a Koopa
		for(int i = 0 ;i<width;i++)
			for(int j = 0 ; j<length;j++)
				if(map[i][j] == 'k' || map[i][j] == 'K')
					numKoopa++;

		// new the arrays
		KoopaX = new int[numKoopa]; // -1 is null
		KoopaY = new int[numKoopa];// -1 is null
		Mood = new int[numKoopa]; // 0 is for k and 1 is for K 0 is null
		KoopaMove = new int[numKoopa];
		KoopaKik = new int[numKoopa];
		loopKoopa = new int[numKoopa];
		Coinfront = new bool[numKoopa];
		// full arrays 
		for(int i = 0 ;i<numKoopa;i++)
		{
			KoopaX[i] = -1;
			KoopaY[i] = -1;
			Mood[i] = -1;
			KoopaMove [i] = 0;
			KoopaKik[i] = 0;
			loopKoopa [i] = 0;
			Coinfront[i] = false;
		}
		int q = 0;
		for(int i = 0 ;i<width;i++)
		{
			for(int j = 0 ; j<length;j++)
				if(map[i][j] == 'k')
				{
					KoopaX[q] = i;
					KoopaY[q] = j;
					Mood[q] = 0;
					q++;
				}
				else if(map[i][j] == 'K')
				{
					KoopaX[q] = i;
					KoopaY[q] = j;
					Mood[q] = 1;
					q++;
				}
		}
	}
	// Animation of Koopa
	void AnimationKoopaRight(int num)
	{
		if(!koopaRighttexture.loadFromFile("C:/Users/User/Documents/Visual Studio 2010/Projects/Mario Game/Mario Game/Pictures/Rkoopa.png"))
			cout<<"cant Load Animation of Koopa Right\n";
		int ans = num % 2;
		if(ans == 0)
		{
			koopaRightsprite.setTextureRect(sf::IntRect(ans*32,0,32,32));
		}
		else if(ans == 1)
		{
			koopaRightsprite.setTextureRect(sf::IntRect(ans*32,0,32,32));
		}
		else
		{
			koopaRightsprite.setTextureRect(sf::IntRect(0,0,32,32));
		}
	}
	void AnimationKoopaLeft(int num)
	{
		int ans = num % 2;
			if(!koopaLefttexture.loadFromFile("C:/Users/User/Documents/Visual Studio 2010/Projects/Mario Game/Mario Game/Pictures/Lkoopa.png"))
				cout<<"cant load a koopa animation \n";

		if(ans == 0)
		{
			koopaLeftsprite.setTextureRect(sf::IntRect(ans*32,0,32,32));
		}
		else if(ans == 1)
		{			
			koopaLeftsprite.setTextureRect(sf::IntRect(ans*32,0,32,32));
		}
		else
		{
			koopaLeftsprite.setTextureRect(sf::IntRect(0,0,32,32));
		}
	}
	// Move Koopa
	void MoveKoopa(char ** map , int length , int width)
	{
		for(int i = 0 ; i < numKoopa ; i ++)
		{
			if(KoopaX[i] != -1 && KoopaY[i]!= -1 && KoopaMove[i] >= 20 && Mood[i] != 2 && Mood[i] != 3)
			{
				KoopaMove[i] = 0 ;
				loopKoopa[i]++ ;
				if(Mood[i] == 1)// K  move rigth
				{
					if((map[KoopaX[i]][KoopaY[i]+1] == '?' || map[KoopaX[i]][KoopaY[i]+1] == '.' || map[KoopaX[i]][KoopaY[i]+1] == 's' || map[KoopaX[i]][KoopaY[i]+1] == 'g' || map[KoopaX[i]][KoopaY[i]+1] == 'G' || map[KoopaX[i]][KoopaY[i]+1] == 'K' || map[KoopaX[i]][KoopaY[i]+1] == 'K' )&& map[KoopaX[i]+1][KoopaY[i]+1] !='.')
					{
						if(map[KoopaX[i]][KoopaY[i]+1] != 's')
						{
							if(map[KoopaX[i]][KoopaY[i]+1] == '?')
							{
								Coinfront[i] = true;
								map[KoopaX[i]][KoopaY[i]]= '.';
								KoopaY[i]++;
							}
							else
							{
								if(Coinfront[i]  == true)
								{
									map[KoopaX[i]][KoopaY[i]+1]= 'K';
									KoopaY[i]++;
									Coinfront[i]  = false;
								}
								else
								{
									map[KoopaX[i]][KoopaY[i]]= '.';
									map[KoopaX[i]][KoopaY[i]+1]= 'K';
									KoopaY[i]++;
								}
							}
						}
						else
						{
							EndGame = true;
							OVER =true;
						}
					}
					else if(map[KoopaX[i]][KoopaY[i]+1] == '#' || map[KoopaX[i]][KoopaY[i]+1] == 'c'|| map[KoopaX[i]][KoopaY[i]+1] == 't'|| KoopaY[i] == length-1)
					{
						Mood[i] = 0;
					}
					else if(map[KoopaX[i]][KoopaY[i]+1]=='.' && map[KoopaX[i]+1][KoopaY[i]+1]=='.')
					{
						Mood[i] = 0;		
					}
					AnimationKoopaRight(loopKoopa[i]);
				}
				else if(Mood[i] == 0 ) // k left
				{
					if(( map[KoopaX[i]][KoopaY[i]-1] == '.'  ||  map[KoopaX[i]][KoopaY[i]-1] == '?' ||  map[KoopaX[i]][KoopaY[i]-1] == 's' || map[KoopaX[i]][KoopaY[i]-1] == 'g' ||  map[KoopaX[i]][KoopaY[i]-1] == 'G' ||  map[KoopaX[i]][KoopaY[i]-1] == 'K' || map[KoopaX[i]][KoopaY[i]-1] == 'k' )&& map[KoopaX[i]+1][KoopaY[i]-1]!='.')
					{
						if(map[KoopaX[i]][KoopaY[i]-1] != 's')
						{
							if(map[KoopaX[i]][KoopaY[i]-1] == '?')
							{
								Coinfront[i] = true;
								map[KoopaX[i]][KoopaY[i]]= '.';
								KoopaY[i]--;
							}
							else
							{
								if(Coinfront[i]  == true)
								{
									map[KoopaX[i]][KoopaY[i]-1]= 'K';
									KoopaY[i]--;
									Coinfront[i]  = false;
								}
								else
								{
									map[KoopaX[i]][KoopaY[i]]= '.';
									map[KoopaX[i]][KoopaY[i]-1]= 'K';
									KoopaY[i]--;
								}
							}
						}
						else
						{
							EndGame = true;
							OVER =true;
						}
					}
					else if( map[KoopaX[i]][KoopaY[i]-1] == '#' || map[KoopaX[i]][KoopaY[i]-1] == 'c'|| map[KoopaX[i]][KoopaY[i]-1] == 't' || KoopaY[i] == 0)
					{			
						Mood[i] = 1;
					}
					else if(map[KoopaX[i]][KoopaY[i]-1]=='.' && map[KoopaX[i]+1][KoopaY[i]-1]=='.')
					{
						Mood[i] = 1;
					}
					AnimationKoopaLeft(loopKoopa[i]);
				}
			}
		}
	}
};
////////////////////////MARIO//////////////////////////////////////////////////
class Mario
{
public:
	int LoopMario;
	bool JumpTrue ,isright; 
	string FileName;
	sf::Clock speed,ClockJump;
	sf::Vector2i mariopos;
	sf::Texture PlayerTexture;
	sf::Sprite PlayerSprite;
	int len,wid;

	// Constructor
	Mario::Mario( int width , int length , char ** map)
	{
		wid = width;
		len = length;
		FileName ="C:/Users/User/Documents/Visual Studio 2010/Projects/Mario Game/Mario Game/Pictures/animation.png";
		isright =true;
		LoopMario = 0;
		JumpTrue= false;
		// Texture  and Sprite
		if(!PlayerTexture.loadFromFile("C:/Users/User/Documents/Visual Studio 2010/Projects/Mario Game/Mario Game/Pictures/mario1.png"))
			cout<<"cant load a player \n";
		PlayerSprite.setTexture(PlayerTexture);
		// marioX and marioY
		for (int i = 0 ; i < width; i++)
		{	
			for (int j = 0; j < length; j++)
				if(map[i][j] == 's')
				{
					mariopos.x= i ;
					mariopos.y = j ;
				}
		}
	}
	// Animation of mario
	void AnimationOfMario(int num)
	{
		if( isright == true)
			FileName = "C:/Users/User/Documents/Visual Studio 2010/Projects/Mario Game/Mario Game/Pictures/ranimation.png";
		else
			FileName = "C:/Users/User/Documents/Visual Studio 2010/Projects/Mario Game/Mario Game/Pictures/lanimation.png";
		int ans = num % 4;
		if(ans == 0)
		{
			if(!PlayerTexture.loadFromFile(FileName))
				cout<<"cant load a player animation \n";
			PlayerSprite.setTextureRect(sf::IntRect(ans*32,0,32,32));
		}
		else if(ans == 1)
		{
			if(!PlayerTexture.loadFromFile(FileName))
				cout<<"cant load a player animation \n";
			PlayerSprite.setTextureRect(sf::IntRect(ans*32,0,32,32));
		}
		else if(ans == 2)
		{
			if(!PlayerTexture.loadFromFile(FileName))
				cout<<"cant load a player animation \n";
			PlayerSprite.setTextureRect(sf::IntRect(ans*33,0,32,32));
		}
		
		else if(ans == 3)
		{
			if(!PlayerTexture.loadFromFile(FileName))
				cout<<"cant load a player animation \n";
			PlayerSprite.setTextureRect(sf::IntRect(ans*32,0,32,32));
		}
		else
		{
			if(!PlayerTexture.loadFromFile(FileName))
				cout<<"cant load a player  animation\n";
			PlayerSprite.setTextureRect(sf::IntRect(64,0,32,32));
		}
	}
	// Move mario
	void move( int width ,int length,char ** map )
	{
		if(sf::Keyboard::isKeyPressed(sf::Keyboard::Space) && JumpTrue == false)
		{
				speed.restart();
				int count =1;
					while( count <= 4 )
					{	
						if( mariopos.x -1>=0 )
						{
							if(map[mariopos.x-1][mariopos.y] == '.' || map[mariopos.x-1][mariopos.y] == '?')
							{
								if(map[mariopos.x-1][mariopos.y] == '?')
									Coin++;
								ClockJump.restart();
								map[mariopos.x][mariopos.y]='.';
								map[(mariopos.x -1<0)?0:mariopos.x -1][mariopos.y]='s';
								count++;
								mariopos.x --;
								JumpTrue = true;
									LoopMario++;
								AnimationOfMario(LoopMario);
							}
							else
								break;
						}

					}
					if(map[mariopos.x -1 ][mariopos.y] == 'c')
					{
						map[mariopos.x -1 ][mariopos.y] = 't';
						map[mariopos.x -2 ][mariopos.y] = '?';
					}
		}
		if(sf::Keyboard::isKeyPressed(sf::Keyboard::Right))
		{
			if(map[mariopos.x][mariopos.y +1] != '#' && map[mariopos.x][mariopos.y +1] !='c' &&  map[mariopos.x][mariopos.y +1] !='t' && speed.getElapsedTime().asSeconds() >0.1)
			{
				if(map[mariopos.x][mariopos.y+1] == 'Y'|| map[mariopos.x][mariopos.y+1] == 'y' ||map[mariopos.x][mariopos.y+1] == 'k'  || map[mariopos.x][mariopos.y+1] == 'K' || map[mariopos.x][mariopos.y+1] == 'G' || map[mariopos.x][mariopos.y+1] == 'g' )
				{
					EndGame =true;
					OVER = true;
				}
				if(map[mariopos.x][mariopos.y +1] == '?')
				{
					Coin++;
				}
				isright =true;
				speed.restart();
				map[mariopos.x][mariopos.y] = '.';
				map[mariopos.x][(mariopos.y+1 == length-1)? mariopos.y : ++ mariopos.y ] = 's';

				if(mariopos.y < k+9 )
				{	
					d--;
					k--;
				}
				d++;
				k++;
				if(d > length -1)
				{
					d= length-1;
					k = length -22;
				}
					LoopMario++;
				AnimationOfMario(LoopMario);
			}
		}
		if(sf::Keyboard::isKeyPressed(sf::Keyboard::Left))
		{
			if(map[mariopos.x][mariopos.y -1] != '#' &&   map[mariopos.x][mariopos.y -1] != 't' && map[mariopos.x][mariopos.y -1] != 'c' && speed.getElapsedTime().asSeconds() >0.1)
			{
				if ( map[mariopos.x][mariopos.y -1] == 'Y'|| map[mariopos.x][mariopos.y -1] == 'y'|| map[mariopos.x][mariopos.y -1] == 'G' || map[mariopos.x][mariopos.y -1] == 'g' || map[mariopos.x][mariopos.y -1] == 'K' || map[mariopos.x][mariopos.y -1] == 'k')
				{
					EndGame =true;
					OVER = true;
				}
				if(map[mariopos.x][mariopos.y-1] == '?')
				{
					Coin++;
				}
				isright =false;
				speed.restart();
				map[mariopos.x][mariopos.y] = '.';
				map[mariopos.x][(mariopos.y-1 < 0)? 0 : --mariopos.y ] = 's';
				if(mariopos.y > d-6 )
				{	
					d++;
					k++;
				}
				d--;
				k--;
				if(k<0)
				{	
					k=0;
					d = 20;
				}
					LoopMario++;
				AnimationOfMario(LoopMario);
			}
		}
	}
	// mario fall
	void fall(bool jump ,int width, char** map , Goomba  goomba , Koopa koopa , Scoot scoot)
	{
		bool kikkoopa = false;
		int keepup = 0 , mood ;
		if( jump == false  )
		{
			if(map[mariopos.x+1][mariopos.y] == '.' || map[mariopos.x+1][mariopos.y] == '?' )
			{
				map[mariopos.x][mariopos.y] = '.';
				for(int h=mariopos.x ; h < width ;h++)
				{
					if( h == width)
					{
						map [ mariopos.x][mariopos.y] = '.';
						EndGame = true;
						OVER = true;
					}
					if(map[h][mariopos.y] == '#' || map[h][mariopos.y] == 'r' || map[h][mariopos.y] == 'c' || map[h][mariopos.y] == 'm' || map[h][mariopos.y] == 't')
					{
						keepup = h;
						break;
					}
				}
				if(keepup > 0 )
				{
					if(map[keepup -1][mariopos.y] =='g' || map[keepup -1][mariopos.y] =='G')
					{
						for(int l = 0 ; l<goomba.numGoomba ; l++)
						{
							if( goomba.GoombaX[l] == keepup-1 && goomba.GoombaY[l] == mariopos.y )
							{
								goomba.GoombaX[l] = -1;
								goomba.GoombaY[l] = -1;
								goomba.Mood[l] = -1;
								map[keepup -1][mariopos.y] = '.';		
							}	
						}
					}
				if(( map[keepup -1][mariopos.y] =='k' || map[keepup -1][mariopos.y] =='K'))
				{
					for(int l = 0 ; l<koopa.numKoopa ; l++)
					{
						if( koopa.KoopaX[l] == keepup-1 && koopa.KoopaY[l] == mariopos.y && koopa.Mood[l] < 2)
						{
							if(koopa.Mood[l] == 1 )
								koopa.Mood[l] = 3;
							else  if (koopa.Mood[l] == 0)
								koopa.Mood[l] = 2;
							kikkoopa = true;
							break;
						}
						else if( koopa.KoopaX[l] == keepup-1 && koopa.KoopaY[l] == mariopos.y )
						{
							kikkoopa = true;
							scoot.ScootX[l] = koopa.KoopaX[l] ;
							scoot.ScootY[l] = koopa.KoopaY[l] ;
							scoot.MoodScoot[l] = koopa.Mood[l] ;
							mood = koopa.Mood[l];
							koopa.KoopaX[l] = -1;
							koopa.KoopaY[l] = -1;
							koopa.Mood[l] = -1;
							if(scoot.MoodScoot[l] == 3 )
								map[keepup -1][mariopos.y] = 'Y';
							else
								map[keepup -1][mariopos.y] = 'y';
						}
					}
				}
				if(kikkoopa == true)
				{
					if (map[keepup-1][mariopos.y + 1] == '.')
					{
						map[keepup-1][mariopos.y+1] = 's';
						mariopos.x = keepup-1;
						mariopos.y ++;
					}
					else
					{
						map[keepup-1][mariopos.y-1] = 's';
						mariopos.x = keepup-1;
						mariopos.y --;
					}
					kikkoopa = false;
				}
				else
				{
					map[keepup-1][mariopos.y] = 's';
					mariopos.x = keepup-1;
				}
				}	
		}
	}
		else if(ClockJump.getElapsedTime().asSeconds() > 0.5)
		{		
			JumpTrue =false;
			map[mariopos.x][mariopos.y] = '.';
			for(int h=mariopos.x ; h < width ;h++)
			{
				if( h == width)
				{
					map [ mariopos.x][mariopos.y] = '.';
					EndGame = true;
					OVER = true;
				}
				if(map[h][mariopos.y] == '#' || map[h][mariopos.y] == 'r' || map[h][mariopos.y] == 'c' || map[h][mariopos.y] == 'm' || map[h][mariopos.y] == 't')
				{
					keepup = h;
					break;
				}
			}
			if(keepup - 1 > 0 )
			{
				if(map[keepup -1][mariopos.y] =='g' || map[keepup -1][mariopos.y] =='G')
				{
					for(int l = 0 ; l<goomba.numGoomba ; l++)
					{
						if(  goomba.GoombaX[l] == keepup-1 && goomba.GoombaY[l] == mariopos.y )
						{
							goomba.GoombaX[l] = -1;
							goomba.GoombaY[l] = -1;
							goomba.Mood[l] = -1;
							map[keepup -1][mariopos.y] = '.';					
						}
					}
				}
				if(( map[keepup -1][mariopos.y] =='k' || map[keepup -1][mariopos.y] =='K'))
				{
					for(int l = 0 ; l<koopa.numKoopa ; l++)
					{
						if( koopa.KoopaX[l] == keepup-1 && koopa.KoopaY[l] == mariopos.y && koopa.Mood[l] < 2)
						{
							if(koopa.Mood[l] == 1 )
								koopa.Mood[l] = 3;
							else if (koopa.Mood[l] == 0)
								koopa.Mood[l] = 2;
							kikkoopa = true;
							mood = koopa.Mood[l];
							break;
						}
						else if( koopa.KoopaX[l] == keepup-1 && koopa.KoopaY[l] == mariopos.y )
						{
							kikkoopa = true;
							scoot.ScootX[l] = koopa.KoopaX[l] ;
							scoot.ScootY[l] = koopa.KoopaY[l] ;
							scoot.MoodScoot[l] = koopa.Mood[l] ;
							mood = koopa.Mood[l];
							koopa.KoopaX[l] = -1;
							koopa.KoopaY[l] = -1;
							koopa.Mood[l] = -1;
							if(scoot.MoodScoot[l] == 3 )
								map[keepup -1][mariopos.y] = 'Y';
							else
								map[keepup -1][mariopos.y] = 'y';
						}
					}
				}
				if(kikkoopa == true)
				{
					if (map[keepup-1][mariopos.y - 1] == '.' && (mood == 1 || mood == 3))
					{
						map[keepup-1][mariopos.y-1] = 's';
						mariopos.x = keepup-1;
						mariopos.y --;
					}
					else if (map[keepup-1][mariopos.y + 1] == '.' && (mood == 0 || mood == 2))
					{
						map[keepup-1][mariopos.y+1] = 's';
						mariopos.x = keepup-1;
						mariopos.y ++;
					}
					else 
					{
						if (map[keepup-1][mariopos.y + 1] == '.')
						{
							map[keepup-1][mariopos.y+1] = 's';
							mariopos.x = keepup-1;
							mariopos.y ++;
						}
						else
						{
							map[keepup-1][mariopos.y-1] = 's';
							mariopos.x = keepup-1;
							mariopos.y --;
						}
					kikkoopa = false;
					}
				}
				else
				{
					map[keepup-1][mariopos.y] = 's';
					mariopos.x = keepup-1;
				}
			}
		}
	}

};
///////////////////////////////////////////////////////////////MAIN///////////////////////////////////////////////////////////////////////////////
int main()
{
	int length;
	int width;
	string Coinstring, Timestring;
	bool coindraw = false , BoxDraw = false , Enddraw = false , Win = false , kikBox = false;
	// lOad a map
	fstream File("map.txt", std::ios::in);
		File >> width >> length;
		char ** map = new char*[width];
		for (int i = 0 ; i < width; i++)
		{
			map[i] = new char[length];
			for (int j = 0; j < length; j++)
			{
				File >> map[i][j];
			}
		}
	d = width;

	// OBJ of Classes
	Mario mario(width,length,map);
	Goomba g(length,width,map);
	Koopa kopa(length,width,map);
	Others Other(width);
	Scoot scoot(kopa.numKoopa);

		for (int i = width -1 ; i >=0; i--)
		{
			for (int j = length -1 ; j>= 0; j--)
			if(map[i][j] == 'e')
				{
					Other.End.x = i ;
					Other.End.y = j ;
					break;
				}
		}
		sf::Texture BrandTexture;
		if(!BrandTexture.loadFromFile("C:/Users/User/Documents/Visual Studio 2010/Projects/Mario Game/Mario Game/Pictures/Brand.png"))
			cout<<"cant load a brand\n";
		sf::Sprite BrandSprite;
		BrandSprite.setTexture(BrandTexture);

		///////////////////////////////////////////////////////////PLAYER NAME /////////////////////////////////////////////////////////////////
	sf::RenderWindow PLAYER(sf::VideoMode(700,400), "PLAYER NAME");
	
	BrandSprite.setPosition( 700 - 100  , 400 - 25);
//		BrandSprite.setPosition(0,0);
	sf::Text Playern;
	sf::Text top ,down;
	top.setFont(Other.font);
	top.setCharacterSize(20);
	top.setColor(sf::Color::Cyan);
	top.setPosition(120,100);
	top.setString(" PLEASE TYPE YOUR NAME : ");

	down.setFont(Other.font);
	down.setCharacterSize(20);
	down.setColor(sf::Color::Blue);
	down.setPosition( 100,300);
	down.setString("press Enter for run a game");


	Playern.setFont(Other.font);
	Playern.setCharacterSize(20);
	Playern.setColor(sf::Color::Red);
	Playern.setPosition( 320,200);

	string NAME;


	while (PLAYER.isOpen())
    {
	PLAYER.clear();
        sf::Event event;
        while (PLAYER.pollEvent(event))
        {
			if (event.type == sf::Event::Closed)
				PLAYER.close();
			if (event.type == sf::Event::TextEntered )
			{
				if(event.text.unicode ==  13 )
				{
					if( NAME == "")
						NAME = "PLAYER";
					PLAYER.close();
				}
				else if(event.text.unicode != 8  && event.text.unicode != 13 )
				NAME += (char)event.text.unicode ;
				else
					NAME =NAME.substr(0,NAME.length()-1);
			}
        }
		PLAYER.draw(BrandSprite);
		Playern.setString(NAME);
		PLAYER.draw(top);
		PLAYER.draw(down);
		PLAYER.draw(Playern);
		PLAYER.display();
	}
	////////////////////////////////////////////WINDOW///////////////////////////////////////////////////////////////
	// Load a texture 
	sf::Texture btexture  ,EndTexture   , WinTexture;
	if(!btexture.loadFromFile("C:/Users/User/Documents/Visual Studio 2010/Projects/Mario Game/Mario Game/Pictures/ground.png"))
		std::cout<<"cant load a ground\n";
	if(!EndTexture.loadFromFile("C:/Users/User/Documents/Visual Studio 2010/Projects/Mario Game/Mario Game/Pictures/Gameover.png"))
		cout<<"cant load a End  \n";
	if(!WinTexture .loadFromFile("C:/Users/User/Documents/Visual Studio 2010/Projects/Mario Game/Mario Game/Pictures/Win.png"))
				cout<<"cant load a Win \n";
	// Sprites
	sf::Sprite bsprite(btexture)  , EndSprite(EndTexture),  WinSprite(WinTexture) ;

	EndSprite.scale((float)(640)/(EndTexture.getSize().y) , (float)(width*32)/(EndTexture.getSize().x));
	WinSprite.scale( (float)(640)/(WinTexture.getSize().y),(float)(width*32)/(WinTexture.getSize().x));
	bsprite.scale(2,2);

	sf::Vector2i screendimension(640 , 32*width);
	
	// Main Window
		sf::RenderWindow window(sf::VideoMode( screendimension.x,screendimension.y), "MARIO GAME!!!");
		BrandSprite.setPosition(  screendimension.x - 100 ,  screendimension.y -25);
	// window is open
		//clock
	sf::Clock clock;
	
    while (window.isOpen())
	{
        sf::Event event;
		while (window.pollEvent(event)){
            if (event.type == sf::Event::Closed)
                window.close();
			if(event.type==sf::Event::KeyPressed && event.key.code==sf::Keyboard::Q)
				window.close();
		}	
	if(EndGame == false)
	{
		// Koopa Clock
		for(int clk = 0 ; clk<kopa.numKoopa; clk++)
		{
			if(kopa.Mood[clk] == 1 || kopa.Mood[clk] == 0)
			{
				kopa.KoopaKik[clk] =0;
				if(kopa.KoopaMove[clk] >=0)
					kopa.KoopaMove[clk]  = kopa.KoopaMove[clk]+ 4;
			}
			else if(kopa.Mood[clk] == 2 || kopa.Mood[clk] == 3)
			{
				kopa.KoopaMove[clk] =0;
				if(kopa.KoopaKik[clk] >=0)
					kopa.KoopaKik[clk] ++;
			}
		}
		//Goomba Clock
		for(int clk = 0 ; clk<g.numGoomba ; clk++)
			g.GoombaClock[clk] = g.GoombaClock[clk] +4;
		//scoot Clock
		for(int clk = 0 ; clk<scoot.numScoot ; clk++)
		{
			if(scoot.MoodScoot[clk] == 2 || scoot.MoodScoot[clk] == 3)
			{
				scoot.ClockScoot[clk] ++;
			}
		}
		mario.move(width, length, map);
		window.clear();
		for (int i = 0 ;i < width; i++)
		{
			for (int j =k ; j <= d+4; j++)
			{
				char stch = map[i][j] ;
				switch(stch){
				case '.':
					bsprite.setTextureRect(sf::IntRect(16,16, 16, 16));
					break;
				case '#':
					if( map[i-1][j] == '#' && map[i][j-1] == '#' && map[i][j+1] == '#' && map[i-1][j+1] != '#' && map[i-1][j-1] !='#')
						bsprite.setTextureRect(sf::IntRect(186,0,16,16));
					else if(map[i-1][j] == '#' && map[i][j-1] == '#' && map[i-1][j-1] != '#' && map[i][j+1] != '#')
						bsprite.setTextureRect(sf::IntRect(46,0,16,16));
					else if(map[i-1][j] == '#' && map[i][j-1] == '#' && map[i-1][j-1] != '#')
						bsprite.setTextureRect(sf::IntRect(35,176, 16, 16));
					else if(map[i-1][j] == '#' && map[i][j+1] == '#' && map[i-1][j+1] != '#' && map[i][j-1]!='#')
						bsprite.setTextureRect(sf::IntRect(141,0,16,16));
					else if(map[i-1][j] == '#' && map[i][j+1] == '#' && map[i-1][j+1] != '#')
						bsprite.setTextureRect(sf::IntRect(18,176, 16, 16));
					else if (map[i-1][j] != '#' && map[i][j-1] != '#' && map[i][j+1] != '#')
						bsprite.setTextureRect(sf::IntRect(0,0,16,16));
					else if(map[i-1][j] != '#' && map[i][j-1] != '#')
						 bsprite.setTextureRect(sf::IntRect(137,99, 16, 16));
					else if(map[i-1][j] != '#' && map[i][j+1] != '#')
						bsprite.setTextureRect(sf::IntRect(171,99, 16, 16));
					else if(map[i-1][j] == '.' || map[i-1][j] == 's' || map[i-1][j] == 'Y' || map[i-1][j] == 'y' || map[i-1][j] == 'G'  ||map[i-1][j] == 'g' || map[i-1][j] == 'k' || map[i-1][j] == 'K')
						bsprite.setTextureRect(sf::IntRect(154,99, 16, 16));
					else if(map[i-1][j] == '#' && map[i][j-1] != '#' && map[i][j+1] !='#')
						bsprite.setTextureRect(sf::IntRect(117,0, 16, 16));
					else if(map[i-1][j] == '#' && map[i][j-1] != '#')
						bsprite.setTextureRect(sf::IntRect(137,116, 16, 16));
					else if(map[i-1][j] == '#' && map[i][j+1] != '#')
						bsprite.setTextureRect(sf::IntRect(171,116, 16, 16));
					else
						bsprite.setTextureRect(sf::IntRect(69, 39, 16, 16));
					break;
				case 't':
							kikBox = true;
							break;
				case'?':
						coindraw = true;
						Other.LoopCoin ++;
						Other.Coinanimation(Other.LoopCoin);
						break;
				case 'c':
						BoxDraw = true;
						break;
				case 'e':
						Enddraw = true;
						break;
				}
				if(kikBox == true)
				{
					kikBox = false;
					Other.kikBoxSprite.setPosition(32*(j-k)- d, 32*i);
					window.draw(Other.kikBoxSprite);
				}
				else if(Enddraw == true)
				{
					Enddraw  = false;
					Other.Endsprite.setPosition(32*(j-k)- d, 32*i);
						window.draw(Other.Endsprite);
				}
				else if(BoxDraw == true)
				{
						BoxDraw = false;
						Other.BoxSprite.setPosition(32*(j-k)- d, 32*i);
						window.draw(Other.BoxSprite);
					}
					else if(coindraw == true)
					{
						coindraw = false;
						Other.CoinSprite.setPosition(32*(j-k)- d, 32*i);
						window.draw(Other.CoinSprite);
					}
					else
					{
						bsprite.setPosition(32*(j-k)- d, 32*i);
						window.draw(bsprite);
					}
			}
		}
		mario.fall(mario.JumpTrue, width, map,g , kopa ,scoot);
		mario.PlayerSprite.setPosition(32*(mario.mariopos.y - k ) -d , 32*mario.mariopos.x);
	
		// draw Goomba
		for(int h = 0 ; h <g.numGoomba ; h++)
		{
			if(g.GoombaX[h] >= 0 && g.GoombaY[h] >= 0)
			{
				g.gsprite.setPosition(32*(g.GoombaY[h] - k ) -d , 32* g.GoombaX[h]);
				window.draw(g.gsprite);
			}
		}

		// draw Koopa
		for(int h = 0 ; h <kopa.numKoopa ; h++)
		{
			if(kopa.KoopaX[h] >= 0 && kopa.KoopaY[h] >= 0 )
			{
				if(kopa.Mood[h] == 1)
				{
					kopa.koopaRightsprite.setPosition(32*(kopa.KoopaY[h] - k ) -d , 32* kopa.KoopaX[h]);
					window.draw(kopa.koopaRightsprite);
				}
				else if( kopa.Mood[h] == 0)
				{
					kopa.koopaLeftsprite.setPosition(32*(kopa.KoopaY[h] - k ) -d , 32* kopa.KoopaX[h]);
					window.draw(kopa.koopaLeftsprite);
				}
				else if(kopa.Mood[h] == 2 || kopa.Mood[h] == 3)
				{
				kopa.KikSprite.setPosition(32*(kopa.KoopaY[h] - k ) -d , 32* kopa.KoopaX[h]);
				window.draw(kopa.KikSprite);
				}
			}
			// pashodane koopa kik shode bad chand sani ye
			if(kopa.KoopaKik[h] >=70 )
			{
				if(kopa.Mood[h] == 3 )
					kopa.Mood[h] = 1;
				else
					kopa.Mood[h] = 0;
			}
		}
				//draw Scoot
		for(int h = 0 ; h <scoot.numScoot ; h++)
		{
			if(scoot.ScootX[h] >= 0 && scoot.ScootY[h] >= 0)
			{
				//cout<< scoot.ScootY[h] <<endl;//scoot.ClockScoot[h]<<endl;
				scoot.ScootSprite.setPosition(32*(scoot.ScootY[h] - k ) -d , 32*scoot.ScootX[h]);
				window.draw(scoot.ScootSprite);
			}
		}
		kopa.MoveKoopa(map,length,width);
		g.MoveGoomba(map,length,width);
		scoot.Move(map,length,width);
		window.draw(mario.PlayerSprite);

		if(mario.mariopos.x == Other.End.x && mario.mariopos.y == Other.End.y )
		{
			EndGame = true;
			Win = true;
		}
		Other.CText.setString("Coin : ");
		Other.TText.setString("Time : ");
		Other.TimeText.setString(std::to_string((long double)clock.getElapsedTime().asSeconds()));
		Other.CoinText.setString(std::to_string((long double)Coin));
      }
	if(OVER == true)
	{
		EndSprite.setTexture(EndTexture);
		EndSprite.setPosition(0,0);
		window.draw(EndSprite);
		
		EndGame = true;
					
	}
	if(Win == true)
	{
		WinSprite.setTexture(WinTexture);
		WinSprite.setPosition(0,0);
		window.draw(WinSprite);
		EndGame = true;
	}
	window.draw(BrandSprite);
	Playern.setPosition(10,10);
	window.draw(Playern);
	window.draw(Other.TText);
	window.draw(Other.CText);
	window.draw(Other.TimeText);
	window.draw(Other.CoinText);
	window.display();
  }
    return 0;
}