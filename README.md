# movies backend app in java spring boot

## MOVIES
### Movie.java
```java
@Document(collection = "movies") // springframework.data.mongodb.core.mapping.Document
@Data //lombok.Data
@AllArgsConstructor // lombok.AllArgsConstructor
@NoArgsConstructor // lombok.NoArgsConstructor
public class Movie {
    @Id // springframework.data.annotation.Id
    private ObjectId id; // bson.types.ObjectId
    private String imdbId;
    private String title;
    private String releaseDate;
    private String trailerLink;
    private String poster;
    private List<String> genres;
    private List<String> backdrops;
    @DocumentReference // springframework.data.mongodb.core.mapping.DocumentReference
    private List<Review> reviewIds; // embedded relationship 1-to-many

}
```

### MovieRepository.java
```java
@Repository // springframework.stereotype.Repository
public interface MovieRepository extends MongoRepository<Movie, ObjectId> {
    // springframework.data.mongodb.repository.MongoRepository
    Optional<Movie> findMovieByImdbId(String imdbId);

}
```

### MovieService.java
```java
@Service // org.springframework.stereotype.Service
public class MovieService {
    @Autowired
    private MovieRepository movieRepository;
    public List<Movie> allMovies(){
        return movieRepository.findAll();
    }

    public Optional<Movie> oneMovie(String imdbId){
        return movieRepository.findMovieByImdbId(imdbId);
    }
}
```

### MovieController.java
```java
@RestController // springframework.web.bind.annotation.RestController
@RequestMapping("api/v1/movies") // springframework.web.bind.annotation.RequestMapping
public class MovieController {
    @Autowired // springframework.beans.factory.annotation.Autowired
    private MovieService movieService;
    @GetMapping // springframework.web.bind.annotation.GetMapping
    public ResponseEntity<List<Movie>> getAllMovies(){ // springframework.http.ResponseEntity
        return new ResponseEntity<List<Movie>>(movieService.allMovies(), HttpStatus.OK);
	// springframework.http.HttpStatus
    }

    @GetMapping("/{imdbId}")
    public ResponseEntity<Optional<Movie>> getOneMovie(@PathVariable String imdbId){
    	// org.springframework.web.bind.annotation.PathVariable
        return new ResponseEntity<Optional<Movie>>(movieService.oneMovie(imdbId), HttpStatus.OK);
    }
}
```

### MoviesApplication.java
```java
@SpringBootApplication // springframework.boot.autoconfigure.SpringBootApplication
@RestController
public class MoviesApplication {

	public static void main(String[] args) {
		SpringApplication.run(MoviesApplication.class, args);
		// springframework.boot.SpringApplication
	}

	@GetMapping("/")
	public String apiRoot(){
		return "Java spring boot project.";
	}

}
```

## REVIEWS
### Review.java
```java
@Document(collection = "reviews")
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Review {
    @Id
    private ObjectId id;
    private String body;

}
```
