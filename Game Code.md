Game Code
    
    using System;

    namespace project
    {
      public class Rand
    {
        public int Run(int min, int max)
        {
            int range = (max - min) + 1;
            Random rng = new Random();
            return min + rng.Next() % range;
        }
      }

    public class Hero
    {
        public string Name;
        private int Strength;
        private int Dexterity;
        private int Intelligence;
        public double Health;
        public double Mana;
        private double InitialMP;

        public Hero(string name, string myclass)
        {
            Name = name;
            switch (myclass)
            {
                case "Knight": Init(15, 10, 5); break;
                case "Assasin": Init(10, 15, 10); break;
                case "Wizard": Init(5, 5, 20); break;
                case "Hunter": Init(12, 12, 11); break;
                default: Init(); break;
            }
        }

        private void Init(int strength = 10, int dexterity = 10, int intelligence = 10)
        {
            this.Strength = strength;
            this.Dexterity = dexterity;
            this.Intelligence = intelligence;
            Health = 50 + strength;
            Mana = (intelligence);
            InitialMP = Mana;
        }

        public int GetStrength() { return this.Strength; }
        public int GetDexterity() { return this.Dexterity; }
        public int GetIntelligence() { return this.Intelligence; }
        public void UpStrength() { this.Strength += 5; this.Health += 5; }
        public void UpDexterity() { this.Dexterity += 5; }
        public void UpIntelligence() { this.Intelligence += 5; this.Mana += (3 * this.Intelligence); }

        public void Attack(Hero enemy)
        {
            Rand rand = new Rand();
            double damage = Strength;

            // Sprawdzenie, czy atak trafił na podstawie zręczności przeciwnika.
            if (rand.Run(0, 100) > enemy.GetDexterity())
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("Bang! -{0} HP -----------------------", damage);
                Console.ResetColor();
                // Zadanie obrażeń przeciwnikowi.
                enemy.Health -= damage;
            }
            else
            {
                Console.ForegroundColor = ConsoleColor.DarkYellow;
                Console.WriteLine("Dodge!-----------------------------");
                Console.ResetColor();
            }
            Console.WriteLine();
        }

        public void LevelUp()
        {
            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.Write("  1:Strength, 2:Dexterity, 3:Intelligence ... ");
            Console.ResetColor();
            int opt = int.Parse(Console.ReadLine());

            switch (opt)
            {
                case 1: UpStrength(); break;
                case 2: UpDexterity(); break;
                case 3:
                    UpIntelligence();
                    Mana = InitialMP;
                    break;
            }
            Console.ForegroundColor = ConsoleColor.Yellow;
            Console.WriteLine("Level Up! --------------------------");
            Console.WriteLine();
            Console.ResetColor();
        }

        public void LightningBolt(Hero enemy)
        {
            if (Mana >= 5)
            {
                double damage = Intelligence;
                enemy.Health -= damage;
                Mana -= 5;
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("Lightning Bolt! -{0} HP -----------------------", damage);
                Console.ResetColor();
            }
            else
            {
                Console.ForegroundColor = ConsoleColor.DarkYellow;
                Console.WriteLine("Casting a spell has Failed - Mana is too low.");
                Console.ResetColor();
            }
            Console.WriteLine();
        }

        public void BloodSteal(Hero enemy)
        {
            if (Mana >= 5)
            {
                double damage = Math.Ceiling(Intelligence / 2.0);
                enemy.Health -= damage;
                Health += damage;
                Mana -= 5;
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("Blood Steal! -{0} HP -------------------", damage);
                Console.WriteLine();
                Console.ResetColor();
            }
            else
            {
                Console.ForegroundColor = ConsoleColor.DarkYellow;
                Console.WriteLine("Casting a spell has Failed - Mana is too low.");
                Console.WriteLine();
                Console.ResetColor();
            }
            Console.WriteLine();
        }
        public void HealingHands()
        {
                double healing = Intelligence;
                Health += healing;
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Healing Hands! +{0} HP -----------------", healing);
                Console.WriteLine();
                Console.ResetColor();
        }
        public void CastSpell(Hero enemy)
        {
            Console.ForegroundColor = ConsoleColor.DarkBlue;
            Console.Write("1: Lightning Bolt, 2: Blood Steal, 3: Healing Hands ... ");
            Console.ResetColor();
            int spellChoice = int.Parse(Console.ReadLine());

            switch (spellChoice)
            {
                case 1: LightningBolt(enemy); break;
                case 2: BloodSteal(enemy); break;
                case 3: HealingHands(); break;
            }
        }
    }


    class Program
    {
        static void Main(string[] args)
        {
            Console.ForegroundColor = ConsoleColor.Magenta;
            Console.WriteLine("Choose your class: 1: Knight, 2: Assasin, 3: Wizard, 4: Hunter");
            Console.ResetColor();
            int playerClassChoice = int.Parse(Console.ReadLine());

            string playerClass = "";
            switch (playerClassChoice)
            {
                case 1: playerClass = "Knight"; break;
                case 2: playerClass = "Assasin"; break;
                case 3: playerClass = "Wizard"; break;
                case 4: playerClass = "Hunter"; break;
                default: playerClass = ""; break;
            }

            Console.Write("Enter your name: ");
            string playerName = Console.ReadLine();
            Hero playerHero = new Hero(playerName, playerClass);

            Console.ForegroundColor = ConsoleColor.Magenta;
            Console.WriteLine("Choose enemy class: 1: Knight, 2: Assasin, 3: Wizard, 4: Hunter");
            Console.ResetColor();
            int enemyClassChoice = int.Parse(Console.ReadLine());

            string enemyClass = "";
            switch (enemyClassChoice)
            {
                case 1: enemyClass = "Knight"; break;
                case 2: enemyClass = "Assasin"; break;
                case 3: enemyClass = "Wizard"; break;
                case 4: enemyClass = "Hunter"; break;
                default: enemyClass = ""; break;
            }
            Console.Write("Enter enemy name: ");
            string enemyName = Console.ReadLine();
            Hero enemyHero = new Hero(enemyName, enemyClass);

            int tour = 1;

            while (playerHero.Health > 0 && enemyHero.Health > 0)
            {
                if (tour == 1)
                {
                    Console.ForegroundColor = ConsoleColor.Green;
                    Console.WriteLine();
                    Console.WriteLine("Your Turn: " + playerHero.Name);
                    Console.ResetColor();
                }
                else
                {
                    Console.ForegroundColor = ConsoleColor.Cyan;
                    Console.WriteLine();
                    Console.WriteLine("Enemy's Turn: " + enemyHero.Name);
                    Console.ResetColor();
                }
                Console.ForegroundColor = ConsoleColor.Magenta;
                Console.Write("1:Attack, 2:Spells, 3:LevelUp ... ");
                Console.ResetColor();
                int opt = int.Parse(Console.ReadLine());

                switch (opt)
                {
                    case 1:
                        if (tour == 1) playerHero.Attack(enemyHero);
                        else enemyHero.Attack(playerHero);
                        break;

                    case 2:
                        if (tour == 1) playerHero.CastSpell(enemyHero);
                        else enemyHero.CastSpell(playerHero);
                        break;

                    case 3:
                        if (tour == 1) playerHero.LevelUp();
                        else enemyHero.LevelUp();
                        break;
                }
                Console.ForegroundColor = ConsoleColor.Green;
                Console.Write(playerHero.Name + " Str:{0} Dex:{1} Int:{2} HP:{3} MP:{4}", playerHero.GetStrength(), playerHero.GetDexterity(), playerHero.GetIntelligence(), playerHero.Health, playerHero.Mana);
                Console.ResetColor();
                Console.WriteLine();
                Console.ForegroundColor = ConsoleColor.Cyan;
                Console.Write(enemyHero.Name + " Str:{0} Dex:{1} Int:{2} HP:{3} MP:{4}", enemyHero.GetStrength(), enemyHero.GetDexterity(), enemyHero.GetIntelligence(), enemyHero.Health, enemyHero.Mana);
                Console.ResetColor();
                Console.WriteLine();
                tour++;
                if (tour > 2) tour = 1;
            }

            if (playerHero.Health <= 0)
            {
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine();
                Console.WriteLine("Enemy wins! {0} is defeated.", playerHero.Name);
                Console.ResetColor();
            }
            else
            {
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine();
                Console.WriteLine("Congratulations! {0} wins the battle.", playerHero.Name);
                Console.ResetColor();
            }
        }
    }
}
