# AFK-Akismet-PHP
A simple PHP5+ implementation of the Akismet anti-spam system for blogs, forums, and other sites.

## Installation
Place the akismet.php file into whatever folder you use for libraries.
In whatever places you think appropriate, add function calls to utlize the API to check for spam in submissions.
Obtain a Wordpress API Key from https://akismet.com/

## Examples

### Validating API Key
	function test_api_key()
	{
		require_once( 'lib/akismet.php' );
		$akismet = new Akismet( $site_url, $wordpress_api_key );

		$response = $akismet->is_key_valid() ? 'Key is Valid!' : 'Key is Invalid!';
		echo $response;
	}

### See if a comment is spam
	function is_comment_spam( $blog_comment )
	{
		require_once( 'lib/akismet.php' );
		$akismet = new Akismet( $site_url, $wordpress_api_key );

		$akismet->set_comment_author( $blog_comment['author'] );
		$akismet->set_comment_author_email( $blog_comment['user_email'] );
		$akismet->set_comment_author_url( $blog_comment['user_url'] );
		$akismet->set_comment_content( $blog_comment['comment_text'] );
		$akismet->set_comment_type( 'comment' );

		if( $akismet->is_this_spam() ) {
			echo( 'This was spam!' );
			// Recommend storing in a database table for moderator approval.
		} else {
			echo( 'This wasn't spam.' );
			// Store the comment in your database as normal.
		}
	}

### Submit a false negative - aka "Submit Spam"
	function submit_spam( $blog_comment )
	{
		require_once( 'lib/akismet.php' );
		$akismet = new Akismet( $site_url, $wordpress_api_key );

		$akismet->set_comment_author( $blog_comment['author'] );
		$akismet->set_comment_author_email( $blog_comment['user_email'] );
		$akismet->set_comment_author_url( $blog_comment['user_url'] );
		$akismet->set_comment_content( $blog_comment['comment_text'] );

		// Strongly recommended to report the poster's original data or Akismet will not be able to learn from its mistakes.
		$akismet->set_comment_ip( $blog_comment['user_ip'] );
		$akismet->set_comment_referrer( $blog_comment['HTTP_REFERER'] );
		$akismet->set_comment_useragent( $blog_comment['HTTP_USER_AGENT'] );
		$akismet->set_comment_time( $blog_comment['date'] );
		$akismet->set_comment_type( 'comment' );

		$akismet->submit_spam();
	}

### Submit a false positive - aka "Submit Ham"
	function submit_ham( $blog_comment )
	{
		require_once( 'lib/akismet.php' );
		$akismet = new Akismet( $site_url, $wordpress_api_key );

		$akismet->set_comment_author( $blog_comment['author'] );
		$akismet->set_comment_author_email( $blog_comment['user_email'] );
		$akismet->set_comment_author_url( $blog_comment['user_url'] );
		$akismet->set_comment_content( $blog_comment['comment_text'] );

		// Strongly recommended to report the poster's original data or Akismet will not be able to learn from its mistakes.
		$akismet->set_comment_ip( $blog_comment['user_ip'] );
		$akismet->set_comment_referrer( $blog_comment['HTTP_REFERER'] );
		$akismet->set_comment_useragent( $blog_comment['HTTP_USER_AGENT'] );
		$akismet->set_comment_time( $blog_comment['date'] );
		$akismet->set_comment_type( 'comment' );

		$akismet->submit_ham();
	}

### Projects Using This Code

AFKTrack (https://github.com/Arthmoor/AFKTrack)
Sandbox (https://github.com/Arthmoor/Sandbox)
