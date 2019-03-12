# AFK-Akismet-PHP
A simple PHP5+ implementation of the Akismet anti-spam system for blogs, forums, and other sites.

## Installation
Place the akismet.php file into whatever folder you use for libraries.
In whatever places you think appropriate, add function calls to utlize the API to check for spam in submissions.

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
	} else {
		echo( 'This wasn't spam.' );
	}
}
