import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;
import java.util.Scanner;


public class ProjectA2 {
    public static void main(String[] args) {
        Menu menu = new Menu();
        menu.showMenu();
    }
}


class Media {
    private String title;
    private String creator;
    private double duration; // in minutes
    private String releaseDate;


    public Media(String title, String creator, double duration, String releaseDate) {
        this.title = title;
        this.creator = creator;
        this.duration = duration;
        this.releaseDate = releaseDate;
    }

    public String getTitle() { return title; }
    public String getCreator() { return creator; }
    public double getDuration() { return duration; }
    public String getReleaseDate() { return releaseDate; }

    public void setTitle(String title) { this.title = title; }
    public void setCreator(String creator) { this.creator = creator; }
    public void setDuration(double duration) { this.duration = duration; }
    public void setReleaseDate(String releaseDate) { this.releaseDate = releaseDate; }

 
    public String getDetails() {
        return "Title: " + title + "\nCreator: " + creator +
               "\nDuration: " + duration + " mins" +
               "\nRelease Date: " + releaseDate;
    }

    public void play() {
        System.out.println("Playing media: " + title);
    }
}


interface Playable {
    void play();
    void pause();
    void stop();


    default void adjustVolume(int level) {
        System.out.println("Volume adjusted to " + level);
    }
}


class Song extends Media implements Playable {
    private String genre;

    public Song(String title, String creator, double duration, String releaseDate, String genre) {
        super(title, creator, duration, releaseDate);
        this.genre = genre;
    }

    
    @Override
    public void play() {
        System.out.println("Playing song: " + getTitle() + " by " + getCreator());
    }

    @Override
    public void pause() {
        System.out.println("Pausing song: " + getTitle());
    }

    @Override
    public void stop() {
        System.out.println("Stopping song: " + getTitle());
    }

    
    public void displayDetails() {
        System.out.println(getDetails() + "\nGenre: " + genre);
    }


    public void displayDetails(boolean verbose) {
        if (verbose) {
            System.out.println("Detailed Song Information:\n" + getDetails() + "\nGenre: " + genre);
        } else {
            displayDetails();
        }
    }

    public String getGenre() { return genre; }
    public void setGenre(String genre) { this.genre = genre; }
}

class Podcast extends Media implements Playable {
    private int episodeNumber;
    
    public Podcast(String title, String creator, double duration, String releaseDate, int episodeNumber) {
        super(title, creator, duration, releaseDate);
        this.episodeNumber = episodeNumber;
    }

    @Override
    public void play() {
        System.out.println("Playing podcast: " + getTitle() + " (Episode " + episodeNumber + ")");
    }
    
    @Override
    public void pause() {
        System.out.println("Pausing podcast: " + getTitle());
    }
    
    @Override
    public void stop() {
        System.out.println("Stopping podcast: " + getTitle());
    }
    
    public int getEpisodeNumber() { return episodeNumber; }
    public void setEpisodeNumber(int episodeNumber) { this.episodeNumber = episodeNumber; }

    
    @Override
    public String getDetails() {
        return super.getDetails() + "\nEpisode: " + episodeNumber;
    }
}

class Playlist {
    private String name;
    private List<Playable> items;

    public Playlist(String name) {
        this.name = name;
        this.items = new ArrayList<>();
    }

    public void addItem(Playable item) {
        items.add(item);
        System.out.println("Item added to playlist: " + name);
    }

    public void removeItem(Playable item) {
        items.remove(item);
        System.out.println("Item removed from playlist: " + name);
    }

    public void displayPlaylist() {
        System.out.println("Playlist: " + name);
        for (Playable item : items) {
            if (item instanceof Media) {
                System.out.println(((Media) item).getDetails());
            }
        }
    }
    
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public List<Playable> getItems() { return items; }
}


class Library {
    private List<Song> songs;
    private List<Podcast> podcasts;
    private List<Playlist> playlists;

    public Library() {
        songs = new ArrayList<>();
        podcasts = new ArrayList<>();
        playlists = new ArrayList<>();
    }

    
    public void addSong(Song song) {
        songs.add(song);
        System.out.println("Song added: " + song.getTitle());
    }

    public void removeSong(Song song) {
        songs.remove(song);
        System.out.println("Song removed: " + song.getTitle());
    }

    public Song getSongByTitle(String title) {
        for (Song song : songs) {
            if (song.getTitle().equalsIgnoreCase(title)) {
                return song;
            }
        }
        return null;
    }

    public List<Song> getSongs() { return songs; }

   
    public void addPodcast(Podcast podcast) {
        podcasts.add(podcast);
        System.out.println("Podcast added: " + podcast.getTitle());
    }

    public void removePodcast(Podcast podcast) {
        podcasts.remove(podcast);
        System.out.println("Podcast removed: " + podcast.getTitle());
    }

    public Podcast getPodcastByTitle(String title) {
        for (Podcast podcast : podcasts) {
            if (podcast.getTitle().equalsIgnoreCase(title)) {
                return podcast;
            }
        }
        return null;
    }

    public List<Podcast> getPodcasts() { return podcasts; }

    
    public void addPlaylist(Playlist playlist) {
        playlists.add(playlist);
        System.out.println("Playlist added: " + playlist.getName());
    }

    public void removePlaylist(Playlist playlist) {
        playlists.remove(playlist);
        System.out.println("Playlist removed: " + playlist.getName());
    }

    public Playlist getPlaylistByName(String name) {
        for (Playlist playlist : playlists) {
            if (playlist.getName().equalsIgnoreCase(name)) {
                return playlist;
            }
        }
        return null;
    }

    public List<Playlist> getPlaylists() { return playlists; }
}


class User {
    private String username;
    private List<Song> likedSongs;
    private List<String> favoriteGenres;

    public User() {
        username = "";
        likedSongs = new ArrayList<>();
        favoriteGenres = new ArrayList<>();
    }

    public void login() {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter your username: ");
        username = scanner.nextLine();
        System.out.println("Welcome, " + username + "!");
    }

    public String getUsername() { return username; }

    public void addLikedSong(Song song) {
        likedSongs.add(song);
        System.out.println("Liked song: " + song.getTitle());
    }

    public void removeLikedSong(Song song) {
        likedSongs.remove(song);
        System.out.println("Removed liked song: " + song.getTitle());
    }

    public List<Song> getLikedSongs() { return likedSongs; }

    public void addFavoriteGenre(String genre) {
        favoriteGenres.add(genre);
        System.out.println("Added favorite genre: " + genre);
    }

    public List<String> getFavoriteGenres() { return favoriteGenres; }

    public void displayPreferences() {
        System.out.println("Favorite Genres: " + favoriteGenres);
        System.out.println("Liked Songs:");
        for (Song song : likedSongs) {
            System.out.println(" - " + song.getTitle());
        }
    }
}


class Sorter {
    public static void sortSongsByTitle(List<Song> songs) {
        Collections.sort(songs, new Comparator<Song>() {
            @Override
            public int compare(Song s1, Song s2) {
                return s1.getTitle().compareToIgnoreCase(s2.getTitle());
            }
        });
        System.out.println("Songs sorted by title.");
    }

    public static void sortSongsByDuration(List<Song> songs) {
        Collections.sort(songs, new Comparator<Song>() {
            @Override
            public int compare(Song s1, Song s2) {
                return Double.compare(s1.getDuration(), s2.getDuration());
            }
        });
        System.out.println("Songs sorted by duration.");
    }
}

class Filter {
    public static List<Song> filterSongsByGenre(List<Song> songs, String genre) {
        List<Song> filteredSongs = new ArrayList<>();
        for (Song song : songs) {
            if (song.getGenre().equalsIgnoreCase(genre)) {
                filteredSongs.add(song);
            }
        }
        System.out.println("Filtered songs by genre: " + genre);
        return filteredSongs;
    }
}


class Menu {
    private Library library;
    private User user;
    private Scanner scanner;

    public Menu() {
        library = new Library();
        user = new User();
        scanner = new Scanner(System.in);

        library.addSong(new Song("Imagine", "John Lennon", 3.1, "1971", "Rock"));
        library.addSong(new Song("Yesterday", "The Beatles", 2.5, "1965", "Rock"));
        library.addSong(new Song("Bohemian Rhapsody", "Queen", 5.9, "1975", "Rock"));
        library.addSong(new Song("Shape of You", "Ed Sheeran", 4.2, "2017", "Pop"));
        library.addPodcast(new Podcast("The Daily", "The New York Times", 30, "2023", 100));
    }

    public void showMenu() {
        int option = 0;
        do {
            System.out.println("\n=== Spotify Software Menu ===");
            System.out.println("1. Login");
            System.out.println("2. Display All Songs");
            System.out.println("3. Add a Song");
            System.out.println("4. Remove a Song");
            System.out.println("5. Retrieve a Song by Title");
            System.out.println("6. Sort Songs by Title");
            System.out.println("7. Sort Songs by Duration");
            System.out.println("8. Filter Songs by Genre");
            System.out.println("9. Display User Preferences");
            System.out.println("10. Like a Song");
            System.out.println("11. Exit");
            System.out.print("Choose an option: ");

            try {
                option = Integer.parseInt(scanner.nextLine());
            } catch (NumberFormatException e) {
                System.out.println("Please enter a valid number.");
                continue;
            }

            switch (option) {
                case 1:
                    user.login();
                    break;
                case 2:
                    displayAllSongs();
                    break;
                case 3:
                    addSong();
                    break;
                case 4:
                    removeSong();
                    break;
                case 5:
                    retrieveSong();
                    break;
                case 6:
                    Sorter.sortSongsByTitle(library.getSongs());
                    break;
                case 7:
                    Sorter.sortSongsByDuration(library.getSongs());
                    break;
                case 8:
                    filterSongsByGenre();
                    break;
                case 9:
                    user.displayPreferences();
                    break;
                case 10:
                    likeSong();
                    break;
                case 11:
                    System.out.println("Exiting application...");
                    break;
                default:
                    System.out.println("Invalid option. Try again.");
            }
        } while (option != 11);
    }

    private void displayAllSongs() {
        List<Song> songs = library.getSongs();
        if (songs.isEmpty()) {
            System.out.println("No songs in the library.");
        } else {
            for (Song song : songs) {
                song.displayDetails();
                System.out.println("---------------------------");
            }
        }
    }

    private void addSong() {
        try {
            System.out.print("Enter song title: ");
            String title = scanner.nextLine();
            System.out.print("Enter song creator/artist: ");
            String creator = scanner.nextLine();
            System.out.print("Enter song duration (in minutes): ");
            double duration = Double.parseDouble(scanner.nextLine());
            System.out.print("Enter song release date: ");
            String releaseDate = scanner.nextLine();
            System.out.print("Enter song genre: ");
            String genre = scanner.nextLine();

            Song newSong = new Song(title, creator, duration, releaseDate, genre);
            library.addSong(newSong);
        } catch (NumberFormatException e) {
            System.out.println("Invalid input for duration. Please enter a valid number.");
        }
    }

    private void removeSong() {
        System.out.print("Enter the title of the song to remove: ");
        String title = scanner.nextLine();
        Song song = library.getSongByTitle(title);
        if (song != null) {
            library.removeSong(song);
        } else {
            System.out.println("Song not found.");
        }
    }

    private void retrieveSong() {
        System.out.print("Enter the title of the song to retrieve: ");
        String title = scanner.nextLine();
        Song song = library.getSongByTitle(title);
        if (song != null) {
            System.out.println("Song found:");
            song.displayDetails();
        } else {
            System.out.println("Song not found.");
        }
    }

    private void filterSongsByGenre() {
        System.out.print("Enter genre to filter by: ");
        String genre = scanner.nextLine();
        List<Song> filteredSongs = Filter.filterSongsByGenre(library.getSongs(), genre);
        if (filteredSongs.isEmpty()) {
            System.out.println("No songs found for genre: " + genre);
        } else {
            for (Song song : filteredSongs) {
                song.displayDetails();
                System.out.println("---------------------------");
            }
        }
    }

    private void likeSong() {
        System.out.print("Enter the title of the song you want to like: ");
        String title = scanner.nextLine();
        Song song = library.getSongByTitle(title);
        if (song != null) {
            user.addLikedSong(song);
        } else {
            System.out.println("Song not found.");
        }
    }
}


