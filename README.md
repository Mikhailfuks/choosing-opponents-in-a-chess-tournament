using System;

public class Player
{
    public string Name { get; set; }
    public int Score { get; set; }
    public int ID { get; set; }

    public Player(string name, int id)
    {
        Name = name;
        ID = id;
        Score = 0; // Starts with 0 score
    }

    public override string ToString()
    {
        return $"{Name} (ID: {ID}) - Score: {Score}";
    }
}

using System;
using System.Collections.Generic;
using System.Linq;

public class Tournament
{
    private List<Player> players;

    public Tournament()
    {
        players = new List<Player>();
    }

    public void RegisterPlayer(string name)
    {
        int id = players.Count + 1; // Simple ID assignment
        players.Add(new Player(name, id));
        Console.WriteLine($"Player {name} registered successfully with ID: {id}.");
    }

    public void UpdateScore(int id, int score)
    {
        Player player = players.FirstOrDefault(p => p.ID == id);
        if (player != null)
        {
            player.Score = score;
            Console.WriteLine($"Score updated for {player.Name}. New Score: {score}");
        }
        else
        {
            Console.WriteLine("Player not found!");
        }
    }

    public void PairPlayers()
    {
        // Sorting players by score descending
        var sortedPlayers = players.OrderByDescending(p => p.Score).ToList();
        
        Console.WriteLine("Pairing players:");
        for (int i = 0; i < sortedPlayers.Count; i += 2)
        {
            if (i + 1 < sortedPlayers.Count)
            {
                Console.WriteLine($"{sortedPlayers[i]} vs {sortedPlayers[i + 1]}");
            }
        }

        if (sortedPlayers.Count % 2 != 0)
        {
            Console.WriteLine($"{sortedPlayers[sortedPlayers.Count - 1]} has no opponent.");
        }
    }

    public void ListPlayers()
    {
        Console.WriteLine("Players in tournament:");
        foreach (var player in players)
        {
            Console.WriteLine(player);
        }
    }
}

using System;

public class Program
{
    public static void Main(string[] args)
    {
        Tournament tournament = new Tournament();
        bool running = true;

        while (running)
        {
            Console.WriteLine("\nChess Tournament Management");
            Console.WriteLine("1. Register Player");
            Console.WriteLine("2. Update Player Score");
            Console.WriteLine("3. Pair Players");
            Console.WriteLine("4. List Players");
            Console.WriteLine("5. Exit");

            Console.Write("Choose an option: ");
            var choice = Console.ReadLine();

            switch (choice)
            {
                case "1":
                    Console.Write("Enter Player Name: ");


                    string name = Console.ReadLine();
                    tournament.RegisterPlayer(name);
                    break;

                case "2":
                    Console.Write("Enter Player ID to Update Score: ");
                    if (int.TryParse(Console.ReadLine(), out int id))
                    {
                        Console.Write("Enter New Score: ");
                        if (int.TryParse(Console.ReadLine(), out int score))
                        {
                            tournament.UpdateScore(id, score);
                        }
                    }
                    break;

                case "3":
                    tournament.PairPlayers();
                    break;

                case "4":
                    tournament.ListPlayers();
                    break;

                case "5":
                    running = false;
                    break;

                default:
                    Console.WriteLine("Invalid choice! Please try again.");
                    break;
            }
        }

        Console.WriteLine("Exiting the tournament management system.");
    }
}
