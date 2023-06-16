# create react app

npx create-react-app@latest

# setting up the project structure

# create

/src/Context.js
/src/Home.js
/src/Movies.js
/src/SearchForm.js
/src/SingleMovies.js

# URl For movies

const url =
"https://upload.wikimedia.org/wikipedia/commons/f/fc/No_picture_available.png";

# API Key for Movies

https://www.omdbapi.com/

#DECLARE ENV VARIABLE in the root folder

# React Router version 6

Switch is Replaced by Routes

# movies

import React from "react";
import { useGlobalContext } from "./context";
import { Link } from "react-router-dom";
const url =
"https://upload.wikimedia.org/wikipedia/commons/f/fc/No_picture_available.png";

const Movies = () => {
const { movies, isLoading } = useGlobalContext();

if (isLoading) {
return <div className="loading"></div>;
}

return (
<section className="movies">
{movies.map((movie) => {
// console.log(movie);
const { imdbID: id, Poster: poster, Title: title, Year: year } = movie;

        return (
          <Link to={`/movies/${id}`} key={id} className="movie">
            <article>
              <img src={poster === "N/A" ? url : poster} alt={title} />
              <div className="movie-info">
                <h4 className="title"> {title}</h4>
                <p>{year}</p>
              </div>
            </article>
          </Link>
        );
      })}
    </section>

);
};

export default Movies;

# single movie

import React, { useState, useEffect } from "react";
import { useParams, Link } from "react-router-dom";
import { API_ENDPOINT } from "./context";

const SingleMovie = () => {
const { id } = useParams();
// console.log(id);

const [movie, setMovie] = useState({});
const [isLoading, setLoading] = useState(true);
const [error, setError] = useState({ show: false, msg: "" });

const fetchMovie = async (url) => {
const response = await fetch(url);
const data = await response.json();
// console.log(data);
if (data.Response === "False") {
setError({ show: true, msg: data.Error });
setLoading(false);
} else {
setMovie(data);
setLoading(false);
}
};
useEffect(() => {
fetchMovie(`${API_ENDPOINT}&i=${id}`);
}, [id]);

if (isLoading) {
return <div className="loading"></div>;
}
if (error.show) {
return (
<div className="page-error">
<h1>{error.msg}</h1>
<Link to="/" className="btn">
back to movies
</Link>
</div>
);
}

const { Poster: poster, Title: title, Plot: plot, Year: year } = movie;
return (
<section className="single-movie">
<img src={poster} alt={title} />
<div className="single-movie-info">
<h2>{title}</h2>
<p>{plot}</p>
<h4>{year}</h4>
<Link to="/" className="btn">
Back to movies
</Link>
</div>
</section>
);
};

export default SingleMovie;
