<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Movie Booking System</title>
    <style>
        body { font-family: Arial, sans-serif; }
        .movie-list { display: flex; flex-wrap: wrap; }
        .movie { margin: 10px; padding: 10px; border: 1px solid #ccc; width: 200px; }
        .seats { display: grid; grid-template-columns: repeat(5, 1fr); gap: 5px; margin-top: 20px; }
        .seat { width: 30px; height: 30px; background-color: #ddd; cursor: pointer; text-align: center; line-height: 30px; }
        .seat.selected { background-color: #6c7ae0; }
        .seat.booked { background-color: #ff6b6b; cursor: not-allowed; }
        .confirmation { margin-top: 20px; }
    </style>
</head>
<body>
    <h1>Movie Booking System</h1>
    <div class="movie-selection">
        <h2>Select a Movie</h2>
        <div class="movie-list" id="movieList">
    </div>
    </div>

    <div class="seat-selection" style="display:none;">
        <h2>Select Seats</h2>
        <div class="seats" id="seatingMap">
            
        </div>
        <button onclick="proceedToPayment()">Proceed to Payment</button>
    </div>

    <div class="payment" style="display:none;">
        <h2>Payment</h2>
        <p>Total Price: $<span id="total Price">0</span></p>
        <button onclick="confirmBooking()">Confirm Booking</button>
    </div>

    <div class="confirmation" style="display:none;">
        <h2>Booking Confirmation</h2>
        <p id="confirmationDetails"></p>
    </div>

    <script>
        const movies = [
            { title: "Movie A", genre: "SAIRAT", showtimes: ["12:00 PM", "3:00 PM", "6:00 PM"] },
            { title: "Movie B", genre: "AVENGERS: THE END GAME", showtimes: ["1:00 PM", "4:00 PM", "7:00 PM"] },
            { title: "Movie C", genre: "GOLMAL", showtimes: ["2:00 PM", "5:00 PM", "8:00 PM"] }
        ];

        const seats = Array(30).fill(0);  

        function loadMovies() 
       {
            const movieList = document.getElementById('movieList');
            movies.forEach((movie, index) => 
            {
                const movieElement = document.createElement('div');
                movieElement.className = 'movie';
                movieElement.innerHTML = `<h3>${movie.title}</h3><p>${movie.genre}</p><p>${movie.showtimes.join(', ')}</p><button onclick="selectMovie(${index})">Select</button>`;
                movieList.appendChild(movieElement);
            });
        }

        function selectMovie(index) 
        {
            document.querySelector('.movie-selection').style.display = 'none';
            document.querySelector('.seat-selection').style.display = 'block';
            loadSeats();
        }

        function loadSeats() 
        {
            const seatingMap = document.getElementById('seatingMap');
            seatingMap.innerHTML = '';
            seats.forEach((seat, index) => 
            {
                const seatElement = document.createElement('div');
                seatElement.className = `seat ${seat ? 'booked' : ''}`;
                seatElement.innerText = index + 1;
                seatElement.onclick = () => selectSeat(index);
                seatingMap.appendChild(seatElement);
            });
        }

        function selectSeat(index) 
        {
            if (seats[index]) return; 
            seats[index] = seats[index] ? 0 : 1; 
            loadSeats();
        }

        function proceedToPayment() 
        {
            document.querySelector('.seat-selection').style.display = 'none';
            document.querySelector('.payment').style.display = 'block';
            const selectedSeats = seats.filter(seat => seat).length;
            document.getElementById('totalPrice').innerText = selectedSeats * 10; 
        }

        function confirmBooking() 
        {
            document.querySelector('.payment').style.display = 'none';
            document.querySelector('.confirmation').style.display = 'block';
            const confirmationDetails = `
                Movie: ${movies[0].title}<br>
                Seats: ${seats.map((seat, index) => seat ? index + 1 : '').filter(s => s).join(', ')}<br>
                Total Price: $${document.getElementById('totalPrice').innerText}
            `;
            document.getElementById('confirmationDetails').innerHTML = confirmationDetails;
        }

        window.onload = loadMovies;
    </script>
</body>
</html>
