# Installation

We ue vite for react installation.

## why?

React sobat boilerplate and unwanted file yetat Vite sobat install kela tar tya file yet nahi.

> so it is makes installation faster and saving,
> building product is also fast

## Vite installation

command

```react.js
npm create vite@latest
```

# Technologies

> Here we use scss for styling and we fetch data from TMDB website

## API fetch kashi keli?

TMDB website madhun api key ghe kiva api token ghe

> me api token getla aahe tya website varun and tho token me .env file madhe thevla aahe

> .env file madhe api store kartana VITE*APP_TMDB_TOKEN hai nav de and VITE_APP* ha part compulsory aahe pudhe kahi pn taku shakto

> env file madhe kahi pn change kele ki mala server restart karayla lagtho

> Axios is used to fetch the data from TMDB website

## What is Axios?

Axios is a popular JavaScript library that is commonly used for making HTTP requests from a web browser or Node.js environment. It's often used with React to perform AJAX (Asynchronous JavaScript and XML) requests, which allows your React application to communicate with APIs and retrieve or send data without having to refresh the entire page.

Inside code

```reactjs
export const fetchDataFromApi = async (url, params) => {
    try {
        const { data } = await axios.get(BASE_URL + url, {
            headers,
            params,
        });
        return data;
    } catch (err) {
        console.log(err);
        return err;
    }
};

```

## how to fetch api in react js (popular question) ?

1. Using JavaScript Fetch API
   The JavaScript Fetch API is an inbuilt browser’s native API that gives an easy interface to fetch the data from the network. The simplest way to use fetch() is by taking one argument and the path from where the data is to be fetched and then returning a promise in a JSON object.

2. Using Axios Library
   Axios is an online HTTP library running on node.js that allows us to make various HTTP requests to a given server endpoint. If we see it working, then it uses the http module on the server side, whereas it uses XMLHttpRequests on the browser side. For this example, we are going to use the GET method to fetch and display the data in our React application. Here we don’t need to convert the result into a JSON object; it already comes as a JSON object.

> Here we use 2nd method using axios

> data {data} variable madhe store karto and return karto

# Installation of Redux

## code madhe redux cha code kuthe aahe?

store floder madhe store.js madhe all redux files and state store aahet

## 2 thing need to download for Redux

1. redux toolkit
2. react-redux

> redux install using redux docs (Quickstart) and react chya main.jsx file la connect kar tya redux code thevlelya store folder sobat
> using this code in main.jsx

```reactjs
import { store } from "./store/store";
import { Provider } from "react-redux";
```

1. varcha store and provider cha code madhe use kela aahe
2. app store inside provider and tya provider la store ha props pass kela

```reactjs
ReactDOM.createRoot(document.getElementById("root")).render(
    <Provider store={store}>
        <App />
    </Provider>
);
```

## Slice concept in reactJS

me different different component sathi slice banvu shakto tya slice madhe tya component che sarv states asnar ithe me homeSlice.js navachi slice banvli aahe store folder madhe thev tyamdhe home components che sarv slice aahet

> ele?.action
> yacha meaning hoto optional chaining ele is undefined then pudh cha code action execute hot nahi

# React route setup

> me app.jsx madhe react route setup karnar using
> command :

```
import { BrowserRouter, Routes, Route } from "react-router-dom";
```

and return cha srav code <BrowserRouter> tag chya aata madhe asnar and ithe me pages na routes bonar home route, details route

```
return (
        <BrowserRouter>
            <Header />
            <Routes>
                <Route path="/" element={<Home />} />
                <Route path="/:mediaType/:id" element={<Details />} />
                <Route path="/search/:query" element={<SearchResult />} />
                <Route path="/explore/:mediaType" element={<Explore />} />
                <Route path="*" element={<PageNotFound />} />
            </Routes>
            <Footer />
        </BrowserRouter>
    );
```

> > Route take two props

1. path
2. component

```
<Route path="/" element={<Home />} />
```

> mediaType--->TVShow or movie

> :id ---> tya chi id

```
<Route path="/:mediaType/:id" element={<Details />} />
```

> > Movies or V show cha explore page

```
<Route path="/explore/:mediaType" element={<Explore />} />
```

> > varchya ele peki kontach aala nahi tar not found cha page run honar

```
<Route path="*" element={<PageNotFound />} />
```

# Hero Banner

> > path : herobanner folder madhe HeroBanner.jsx file madhe
> > basic structure banvla ahe

1. Background image sathi and query sathi useState banv
2. user ne je type kele tya quey banvun send kar browser la using useNavigate state from react-router-dom
3. api through data fetch karayla sarkhe sakhe same code lihayla lagel so, me hooks folder madhe useFetch navacha hook banvla and jevha pn lagel tehva tyacha use karnar
4. ya code chya help ne api through data fetch kela

```
import { useEffect, useState } from "react";
import { fetchDataFromApi } from "../utils/api";
const useFetch = (url) => {
    const [data, setData] = useState(null);
    const [loading, setLoading] = useState(null);
    const [error, setError] = useState(null);

    useEffect(() => {
        setLoading("loading...");
        setData(null);
        setError(null);

        fetchDataFromApi(url)
            .then((res) => {
                setLoading(false);
                setData(res);
            })
            .catch((err) => {
                setLoading(false);
                setError("Something went wrong!");
            });
    }, [url]);

    return { data, loading, error };
};

export default useFetch;
```

5. herobanner.jsx madhe ya code snnipets ne data me data and loding variable madhe store karnar and ya cha meaning hoto me upcoming movies fetch kartoy

```
const { data, loading } = useFetch("/movie/upcoming");
```

6. Me data fetch kela aahe upcoming movies cha and tya madhun ek movies cha data and img me select kela and tya la banner page var show kela
   (Math.random()\*20) hai ka karan ek api call sobat 20 result ch yetat and use ?. (optional chaining) to avoid undefined result che error
   setBackground(bg); --> image set on the background

```
  useEffect(() => {
    const bg =
      url.backdrop +
      data?.results?.[Math.floor(Math.random() * 20)]?.backdrop_path;
    setBackground(bg);
  }, [data]);
```

7. Tya fetch keleya img url direct nahi chalnar tya sathi mala (base_url + size + img_url) use karayla lagel
   app.jsx madhe ha code karto te kam
   img chi size original asnar and image three types chya asnar backdrop(home page var background sathi) poster(movie chya poster sathi) and profile(actors chya profile sathi)
   > > Base url me TB chya website varun getla aahe

```
const fetchApiConfig = () => {
    fetchDataFromApi("/configuration").then((res) => {
        console.log(res);
        const url = {
            backdrop: res.images.secure_base_url + "original",
            poster: res.images.secure_base_url + "original",
            profile: res.images.secure_base_url + "original",
        };
        dispatch(getApiConfiguration(url));
    });
};
```

8. Me component folder madhe ek lazyLodeImage navacha folder banvla and tya madhe maza sarv image component asnar apan tya sarv img na lazy load karnar

   > > React Lazy Load Image Component
   > > React Component to lazy load images and other components/elements. Supports IntersectionObserver and includes a HOC to track window scroll position to improve performance.

> > lazyLodeImage madhe img.jsx madhe ha code aahe

```
import React from "react";
<!-- Import component LazyLoadImage from this library "react-lazy-load-image-component-->
import { LazyLoadImage } from "react-lazy-load-image-component";
<!-- css pn component sobat ch aala -->
import "react-lazy-load-image-component/src/effects/blur.css";

const Img = ({ src, className }) => {
    return (
        <LazyLoadImage
            className={className || ""}
            alt=""
            effect="blur"
            src={src}
        />
    );
};
<!-- we export thos component -->
export default Img;
```

9. me component madhe contentWrapper cha folder and tyachya aat madhe tyachi file banvli jya file madhe me pratek img wrap karun thevla so every time jevha component centre karycha asel we use contentWrapper component tag

10. Home page var banner img and khalchya img poster chya madhe <div className="opacity-layer"></div> hai add kele blurr effect yenya sathi

## Hero banner section css

we use flex in some places:
The flex CSS shorthand property is the combination of flex-grow, flex-shrink, and flex-basis properties. It is used to set the length of flexible items. The flex property is much more responsive and mobile-friendly. It is easy to position child elements and the main container.

> > we use mixins --> It is the best feature of scss

```
@mixin ellipsis($line: 2) {
    display: -webkit-box;
    -webkit-line-clamp: $line;
    -webkit-box-orient: vertical;
    overflow: hidden;
    text-overflow: ellipsis;
}
```

1. function sarkha @mixin keyword ne ellipsis(give name to that mixin) and nantar tho use kar jithe lagel tithe asa

```
@mixin xxl {
    @media only screen and (min-width: 1536px) {
        @content;
    }
}
```

2. me style.css madhe mobile sathi css use keli ahe and media query cha us karun tyana PC dathi compatible banvle
3. scss madhe 2 things is better than css
   1. mixins
   2. nested styling (eka class madhe me nested styling ne mutilple elements la styling karu shakto)
      eg.
   ````
   .backdrop-img {
        width: 100%;
        height: 100%;
        position: absolute;
        top: 0;
        left: 0;
        opacity: 0.5;
        overflow: hidden;
        .lazy-load-image-background {
            width: 100%;
            height: 100%;
            img {
                width: 100%;
                height: 100%;
                object-fit: cover;
                object-position: center;
            }
        }
    }```
   ````

# Header Section

1. setShowSearch(true); asnar PC sathi and false asnar mobile sathi opposite for setMobileMenu(false);

```
const openSearch = () => {
    setMobileMenu(false);
    setShowSearch(true);
};

const openMobileMenu = () => {
    setMobileMenu(true);
    setShowSearch(false);
};
```

2. Header section 1min nantar automatically off honar using this code

```
  const searchQueryHandler = (event) => {
    if (event.key === "Enter" && query.length > 0) {
      navigate(`/search/${query}`);
      setTimeout(() => {
        setShowSearch(false);
      }, 1000);
    }
  };
```

3. Me kuthe click kele tar kuthe janar hai ya code madhe mention aahe

```
<div className="logo" onClick={() => navigate("/")}>
    <img src={logo} alt="" />
</div>
<ul className="menuItems">
    <li className="menuItem" onClick={() => navigationHandler("movie")}>
    Movies
    </li>
    <li className="menuItem" onClick={() => navigationHandler("tv")}>
    TV Shows
    </li>
    <li className="menuItem">
    <HiOutlineSearch onClick={openSearch} />
    </li>
</ul>
```

4. varcha code madun response aala and me ya code ne tya page var gelo

```
const navigationHandler = (type) => {
if (type === "movie") {
    navigate("/explore/movie");
} else {
    navigate("/explore/tv");
}
setMobileMenu(false);
};
```

5. header section madhla navbar kadhi remove karaycha te ha code decide karnar
   jar (scroll chi size 200 peksha jast zai then hide it) using this condition we check it window.scrollY > 200
   and 200 peksha chota aahe then execute else block

```
const controlNavbar = () => {
if (window.scrollY > 200) {
    if (window.scrollY > lastScrollY && !mobileMenu) {
    setShow("hide");
    } else {
    setShow("show");
    }
} else {
    setShow("top");
}
setLastScrollY(window.scrollY);
};
```

> > What is mean by useEffect ?

1. [search] ha state variable aahe jar tho change zala tar ya ()=> arrow function la call kar

```
useEffect(() => {
    if (search == true && query.length > 0) {
      navigate(`/search/${query}`);
    }
  }, [search]);
```

# Footer

1. Import icons from react-icons

```
import {
    FaFacebookF,
    FaInstagram,
    FaTwitter,
    FaLinkedin,
} from "react-icons/fa";
```

# Carousel section---> cha switch tab day and week vala

> > Carousel section manje movie che carts so point is te Carousel khup jagi use hotayt so, ashya prakare banv ki te mutiple jagi use hotil

1. Day and week dakhav nara separate component banv switchTabs nava cha
2. switch tab move hotana disto te kase karnar it is div so tyala animation dele aahe using const [left, setLeft] = useState(0); inside switchTabs.jsx file
3. Read comments in codes

```
const activeTab = (tab, index) => {
    <!-- jya index la current aahe tya index la 100 ne multiply kar so that we get next new slide tho purn skip honar -->
    setLeft(index * 100);
    <!-- animation smooth chlay la setTimeout lavla -->
    setTimeout(() => {
        setSelectedTab(index);
    }, 300);
    <!-- new index and tab is sended to onTabChange() method-->
    onTabChange(tab, index);
};
```

4. Read comments in codes

```
const Trending = () => {
    const [endpoint, setEndpoint] = useState("day");

    <!-- useFetch ha inbuilt ha me banvla aahe api fetch karnya sathi so ithe me tho hook use karto and end la endpoint ha state variable use karto tho endpint state variable intially "day" asto -->
    const { data, loading } = useFetch(`/trending/movie/${endpoint}`);

    const onTabChange = (tab) => {
        setEndpoint(tab === "Day" ? "day" : "week");
    };

    return (
        <div className="carouselSection">
            <ContentWrapper>
                <span className="carouselTitle">Trending</span>
                <SwitchTabs data={["Day", "Week"]} onTabChange={onTabChange} />
            </ContentWrapper>
            <Carousel data={data?.results} loading={loading} />
        </div>
    );
};

```

# Carousel section ---> main movies chya cart vala

1. read from code

````
import React, { useRef } from "react";
<!-- left and right che arrow -->
import {
    BsFillArrowLeftCircleFill,
    BsFillArrowRightCircleFill,
} from "react-icons/bs";
<!-- import useNavigate browser varti api call karnya sathi -->
import { useNavigate } from "react-router-dom";
import { useSelector } from "react-redux";
<!-- day format api madhe differnt pn mala tho website var diffrent format madhe pahije so use dayjs inbuilt component -->
import dayjs from "dayjs";

import ContentWrapper from "../contentWrapper/ContentWrapper";
import Img from "../lazyLoadImage/Img";
import PosterFallback from "../../assets/no-poster.png";
import CircleRating from "../circleRating/CircleRating";
import Genres from "../genres/Genres";```
````

## What is useRef?

> > import React, { useRef } from "react"; ---> 1st line in above code

1. useRef ha kontya pn ele la access means kontya pn ele la catch karayla use hoto Eg. document.getElementById js madhe jasa kam karto tasa react js madhe useRef kam karto. below chya code snippet madhe useRef() containerAcess madhe thevla and jo ele acess karaycha tyachya ref madhe pass kela then tho use kela me

```
const containerAcess=useRef();
<div ref={containerAcess}></div>
console.log(containerAcess.current);
```

## Movies che and TV show che cart

```
<div className="carousel">
    <ContentWrapper>
        {title && <div className="carouselTitle">{title}</div>}
        <!-- left arrow che button -->
        <BsFillArrowLeftCircleFill
            className="carouselLeftNav arrow"
            onClick={() => navigation("left")}
        />
        <!-- right arrow che button -->
        <BsFillArrowRightCircleFill
            className="carouselRighttNav arrow"
            onClick={() => navigation("right")}
        />
        <!-- ithe ternary operator use kele aahe. loading varti set Varible cha funtion aahe tho jar false means loading complete then show poster otherwise show skeleton-->je <div className="loadingSkeleton"> ya div chya aat mdhe aahe
        {!loading ? (
            <div className="carouselItems" ref={carouselContainer}>
                {data?.map((item) => {
                    const posterUrl = item.poster_path
                        ? url.poster + item.poster_path
                        : PosterFallback;
                    return (
                        <div
                            key={item.id}
                            className="carouselItem"
                            onClick={() =>
                                navigate(
                                    `/${item.media_type || endpoint}/${
                                        item.id
                                    }`
                                )
                            }
                        >
                            <div className="posterBlock">
                                <Img src={posterUrl} />
                                <CircleRating
                                    rating={item.vote_average.toFixed(
                                        1
                                    )}
                                />
                                <Genres
                                    data={item.genre_ids.slice(0, 2)}
                                />
                            </div>
                            <div className="textBlock">
                                <span className="title">
                                    {item.title || item.name}
                                </span>
                                <span className="date">
                                    {dayjs(item.release_date || item.first_air_date).format(
                                        "MMM D, YYYY"
                                    )}
                                </span>
                            </div>
                        </div>
                    );
                })}
            </div>
        ) : (
            <div className="loadingSkeleton">

            <!-- varti ek function banvla aahe tho ithe use kelay se code in below notes-->
                {skItem()}
                {skItem()}
                {skItem()}
                {skItem()}
                {skItem()}
            </div>
        )}
    </ContentWrapper>
</div>
```

1. Just scss add kela ahe and translateX() animation use kele so, it shows it loading

```
const skItem = () => {
    return (
        <div className="skeletonItem">
            <div className="posterBlock skeleton"></div>
            <div className="textBlock">
                <div className="title skeleton"></div>
                <div className="date skeleton"></div>
            </div>
        </div>
    );
};
```

# Rating component

> > CircleRating navacha component banvla aahe

1. tya madhe ji green colour che round diste je rating show karte te ya react js chya inbuilt component ne kele aahe

```
import React from "react";
<!-- import kela componenent  CircularProgressbar, buildStyles ya name ne-->
import { CircularProgressbar, buildStyles } from "react-circular-progressbar";
<!-- css styling pn import keli -->
import "react-circular-progressbar/dist/styles.css";

import "./style.scss";
<!-- carousel madun CircieRating la props pass kela aahe tjo ithe use kar -->
const CircleRating = ({ rating }) => {
    return (
        <div className="circleRating">
            <CircularProgressbar
            <!-- value fetch karun deli ithe value madhe-->
                value={rating}
                <!-- maxvalue ratingchi kay asnar -->
                maxValue={10}
                text={rating}
                <!-- css obj send kela ahe styling sathi rating nusar color de circle la-->
                styles={buildStyles({
                    pathColor:
                        rating < 5 ? "red" : rating < 7 ? "orange" : "green",
                })}
            />
        </div>
    );
};

```

# Generes component

Inside app.jsx

1. promise all method use kele aahe at a time 2 api (tv , movie) cha result fetch kar and store kar

```
const genresCall = async () => {
    let promises = [];
    let endPoints = ["tv", "movie"];
    let allGenres = {};

    endPoints.forEach((url) => {
      promises.push(fetchDataFromApi(`/genre/${url}/list`));
    });

    const data = await Promise.all(promises);
    console.log(data);
    data.map(({ genres }) => {
      return genres.map((item) => (allGenres[item.id] = item));
    });

    dispatch(getGenres(allGenres));
  };
```

2.  The dispatch method
    The dispatch function accepts an object that represents the type of action we want to execute when it is called. Basically, it sends the type of action to the reducer function to perform its job, which, of course, is updating the state.

```
import React from "react";
import { useSelector } from "react-redux";

import "./style.scss";

const Genres = ({ data }) => {
    const { genres } = useSelector((state) => state.home);

    return (
        <div className="genres">
            {data?.map((g) => {
                if (!genres[g]?.name) return;
                return (
                    <div key={g} className="genre">
                        {genres[g]?.name}
                    </div>
                );
            })}
        </div>
    );
};
```
