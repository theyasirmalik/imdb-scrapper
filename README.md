# imdb-scrapper
PHP IMDB Scrapper class (updated)
/////////////////////////////////////////////////////////////////////////////////////////////////////////
// Free PHP IMDb Scraper API for the new IMDb Template.
// Author: Yasir Malik
// Email: theyasirmalik@gmail.com
// Last Updated: April 20, 2020
/////////////////////////////////////////////////////////////////////////////////////////////////////////
Updated Log:
1. Added SSL Support for IMDB
2. Changed Posters Directories
100% Working
/*
* Try Fetching movie details from IMDB
*/

	 	  header('Content-Type: application/json;charset=utf8');
	 	  ob_start();
		 include "imdb.php";
		 
	 	  /* Get IMDB Movie Link without any string params e.g https://www.imdb.com/title/tt7286456/ */
		  $imdb_link = $this->input->post("link"); 
		  
		  if(!empty($imdb_link) AND strstr($imdb_link, 'https://')) {
			try {
				$output = $this->imdb->scrapeMovieInfo($imdb_link);
				
				
				
				$output = (object) $output;
				
				
				$json['title'] = isset($output->title) ? $output->title : 'Could not get IMDB';
				$json['description'] = isset($output->plot) ? $output->plot : '--';
				$json['directors'] = isset($output->directors) ? $output->directors : '--';
				$actors1 = isset($output->cast) ? $output->cast : '';
				$actors2 = isset($output->stars) ? $output->stars : '';
				$json['actors'] = array_merge($actors1, $actors2);
				$json['release_date'] = isset($output->release_date) ? $output->release_date : '--';
				$json['runtime'] = isset($output->runtime) ? $output->runtime .' mins' : '--';
				$json['thumb'] = isset($output->poster) ? $output->poster : '';
				
				if(isset($output->genres)) {
					$json['genres'] = $output->genres;
				}
				
				echo json_encode($json);
				
			} catch (Exception $e) {
				echo json_encode(array('errorMsg' => $e->getMessage()));
			}
			
		  }else{
		  	  echo json_encode(array('errorMsg' => 'Empty URL/Incorrect IMDB URL'));
		  }
		  ob_end_flush();
